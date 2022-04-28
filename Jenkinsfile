pipeline {
    agent any
    environment {
        JENKINS_NODE_COOKIE="dontkill"
    }
    stages {
        stage("Clean WS") {
            steps {
                cleanWs()
            }
        }
        stage("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/evMarin/spring-petclinic.git'
            }
        }
        stage("Build") {
            steps {
                sh "./gradlew build -x test"
            }
        }
        stage("Run App") {
            steps {
                script {
                    
                        sh "nohup java -jar '${WORKSPACE}/build/libs/spring-petclinic-2.6.0.jar' --server.port=8081 &"
                    
                }
            }
        }
        stage("Health Check") {
            steps {
                script {
                    i = 0
                    while (i != 30) {
                        try {
                            i ++
                            status = sh (script: "sleep 2 && curl -k -s http://localhost:8081 -o /dev/null -w '%{http_code}\n'", returnStdout: true)
                            println("Status: ${status}")
                        break
                        }
                        catch (ex) {
                            println(ex)
                        }
                    }
                }
            }
        }
    }
}
