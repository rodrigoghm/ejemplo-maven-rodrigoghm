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
                env.ALUMNO="Rodrigo Higuera"
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean compile -e"
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
        stage("Paso 2: Test"){
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean test -e"
                }
            }
        }
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean package -e"
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
