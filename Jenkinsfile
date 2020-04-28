pipeline {
    agent any
    stages {
        stage('Lint HTML'){
                steps {
                    sh 'tidy -q -e *.html'
                }
            }
        stage('Upload to AWS') {
            steps {
                retry(2){
                withAWS(region:'us-east-1',credentials:'aws-static'){
                    s3Upload(file:'index.html', bucket:'jenkinsproj', path:'')
                }
                }
            }
        }
        stage('Website Status'){
            steps{
                sh '''
                    status=$(curl -Is http://jenkinsproj.s3-website.us-east-1.amazonaws.com/ | head -n 1)
                    echo "$status"
                '''
            }
        }
    }
}