import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}

pipeline {
    agent any
    stages {
        stage("Paso 1: Compile Code"){
            steps {
                script {
                env.STAGE='Compile Code'
                env.ALUMNO='Rodrigo Higuera'
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean compile -e"
                }
            }
            post{
				success{
					slackSend color: 'good', message: "Build Success [${env.ALUMNO}] [${JOB_NAME}] Ejecucion Exitosa en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
				failure{
					slackSend color: 'danger', message: "Build Failure [${env.ALUMNO}] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
			}
        }
        stage("Paso 2: Test"){
            steps {
                script {
                env.STAGE='Test'
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean test -e"
                }
            }
            post{
				success{
					slackSend color: 'good', message: "Build Success [${env.ALUMNO}] [${JOB_NAME}] Ejecucion Exitosa en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
				failure{
					slackSend color: 'danger', message: "Build Failure [${env.ALUMNO}] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
			}
        }
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                env.STAGE='Build Jar'
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean package -e"
                }
            }
            post{
				success{
					slackSend color: 'good', message: "Build Success [${env.ALUMNO}] [${JOB_NAME}] Ejecucion Exitosa en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
				failure{
					slackSend color: 'danger', message: "Build Failure [${env.ALUMNO}] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
			}
        }
        stage("Paso 4: Run App"){
            steps {
                script {
                env.STAGE='Run And Test App'
                sh "echo 'Run App'"
                sh "nohup bash mvnw spring-boot:run & >/dev/null"
                sh "sleep 15 && curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'"
                }
            }
            post{
				success{
					slackSend color: 'good', message: "Build Success [${env.ALUMNO}] [${JOB_NAME}] Ejecucion Exitosa en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
				failure{
					slackSend color: 'danger', message: "Build Failure [${env.ALUMNO}] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
			}
        }
        stage("Paso 5: Test Postman"){
            steps {
                script {
                env.STAGE='Test Postman'
                sh "newman run ./postman/maven2.postman_collection.json  -n 3  --delay-request 500"
                }
            }
            post{
				success{
					slackSend color: 'good', message: "Build Success [${env.ALUMNO}] [${JOB_NAME}] Ejecucion Exitosa en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
				failure{
					slackSend color: 'danger', message: "Build Failure [${env.ALUMNO}] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
			}
        }
        stage("Paso 6: Stop App"){
            steps {
                script {
                env.STAGE='Stop App'
                sh '''
                        echo 'Process Spring Boot Java: ' $(pidof java | awk '{print $1}')  
                        sleep 20
                        kill -9 $(pidof java | awk '{print $1}')
                    '''
                }
            }
            post{
				success{
					slackSend color: 'good', message: "Build Success [${env.ALUMNO}] [${JOB_NAME}] Ejecucion Exitosa en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
				failure{
					slackSend color: 'danger', message: "Build Failure [${env.ALUMNO}] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack-new'
				}
			}
        }
    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
        success {
            sh "echo 'fase success'"
        }

        failure {
            sh "echo 'fase failure'"
        }
    }
}