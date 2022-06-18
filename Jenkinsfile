pipeline {
    agent any
    tools {
        maven 'maven386'
    }
    stages {
        stage('Validate') {
            steps {
                dir('Servicios/Curso-Microservicios') {
                    withSonarQubeEnv('SonarServer') {
                        sh "mvn clean package sonar:sonar \
                            -Dsonar.projectKey=22_MyCompany_Microservice \
                            -Dsonar.projectName=22_MyCompany_Microservice \
                            -Dsonar.sources=src/main \
                            -Dsonar.coverage.exclusions=**/*TO.java,**/*DO.java,**/curso/web/**/*,**/curso/persistence/**/*,**/curso/commons/**/*,**/curso/model/**/* \
                            -Dsonar.coverage.jacoco.xmlReportPaths=microservicio-web/target/site/jacoco/jacoco.xml"
                    }
                }
            }
        }

        stage('Compile') {
            steps {
                dir('Servicios/Curso-Microservicios') {
                    sh 'docker build -t microservicio .'
                }
            }
        }

        stage('Push images') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-nexus', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'
                    sh 'docker push microservicio:lastest'
                 }
            }
        }
    }
}
