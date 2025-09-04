pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Github hook trigger 3'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                sh 'aws s3 cp /var/lib/jenkins/workspace/JenkinsPipeline/index.html s3://rinoue-test-env-jenkins/ --region ap-northeast-1'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                script {
                    def url = sh(script: "aws s3 presign s3://rinoue-test-env-jenkins/index.html --expires-in 60 --region ap-northeast-1", returnStdout: true).trim()
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' '$url'", returnStdout: true)

                    if (response == '200') {
                        echo 'Test OK'
                    } else {
                        echo response
                        error 'Test NG'
                    }
                }
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
                sh 'aws s3 cp /var/lib/jenkins/workspace/JenkinsPipeline/index.html s3://rinoue-prod-env-jenkins/ --region ap-northeast-1'
            }
        }
    }
}
