pipeline{
    agent any
    environment {
        JENKINS_NODE_COOKIE="dontkill"
    }
    stages{
        stage("Clean WS"){
            steps{
                cleanWs()
            }
        }
        stage("Git checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/evMarin/spring-petclinic.git'
            }
        }
        stage("Build"){
            steps{
                sh "./gradlew build -x test"
            }
        }
        stage("Test"){
            steps{
                sh "./gradlew test"
            }
        }
        stage("RUN App"){
            steps{
                script{
                    
                        sh "nohup java -jar '${WORKSPACE}/build/libs/spring-petclinic-2.6.0.jar' --server.port=8081 &"
                    
                }
            }
        }
        stage("CURL request"){
            steps{
                script{
                    sh "curl -k -f --retry 40 --retry-delay 1 --retry-max-time 360 http://localhost:8081 -o /dev/null -w '%{http_code}\n'"
                }
            }
        }
    }
}
