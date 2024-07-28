pipeline {
    agent any
   
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('git-checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ChamankarS/to-do-app.git'            }
            }
        }

    stage('Sonar Analysis') {
            steps {
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=URL_OF_SONARQUBE -Dsonar.login=TOKEN_OF_SONARQUBE -Dsonar.projectName=to-do-app \
                   -Dsonar.sources=. \
                   -Dsonar.projectKey=to-do-app '''
               }
            }
           
     stage('OWASP Dependency Check') {
            steps {
               dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
               }
           }
     stage('Docker Build & Push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '3565399f-2316-40d6-8070-3c02881066c1', toolName: 'docker') {
                        sh "docker build -t todoapp:latest -f backend/Dockerfile . "
                        sh "docker tag todoapp:latest chamankarsahil/todoapp:latest "
                        sh "docker push chamankarsahil/todoapp:latest "
                    }
                }
            }
        }

          
             
      stage('trivy') {
            steps {
               sh " trivy username/todoapp:latest"
            }
        }
        stage('Deploy to Docker') {
            steps {
               script{
                    withDockerRegistry(credentialsId: '3565399f-2316-40d6-8070-3c02881066c1', toolName: 'docker') {
                    sh "docker run -d --name to-do-app -p 4000:4000 chamankarsahil/todoapp:latest "
                 }
               }
            }
        }

    }
}
