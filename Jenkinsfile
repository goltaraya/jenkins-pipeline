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
        stage("Teste da aplicação"){
            steps{
                sh "test-app.sh"
            }
        }
    }
}