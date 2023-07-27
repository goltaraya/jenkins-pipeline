pipeline {
    agent any
    stages{
        stage("Build da imagem docker"){
            steps {
                sh "docker build -t devops/app ."
            }
        }

        stage("Subir docker compose"){
            steps{
                sh "docker compose up -d"
            }
                
        }

        stage("Sleep para subida dos containers"){
            steps{
                sh "sleep 10"
            }
        }
        stage("SonarQube validation"){
            steps{
                script{
                    scannerHome = tool 'sonar-scanner';
                    withSonarQubeEnv('sonar-server'){
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=redis-app -Dsonar.sources=. -Dsonar.host.url=${env.SONAR_HOST_URL} -Dsonar.login=${env.SONAR_AUTH_TOKEN}"
                    }
                }
            }
        }
        stage("Quality Gate"){
            steps{
                waitForQualityGate abortPipeline: true
            }
        }      
        stage("Teste da aplicação"){
            steps{
                sh "chmod +x test-app.sh"
                sh "./test-app.sh"
            }
        }
        stage("Destruindo containers"){
            steps{
                sh "docker compose down"
            }
        }
    }
}
