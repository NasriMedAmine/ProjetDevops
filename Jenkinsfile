pipeline {
    agent any
    
    triggers {
        // Déclenche le build dès qu'un changement est détecté dans le dépôt Git.
        pollSCM('H/1 * * * *')
    }

    environment {
        // Define any global environment variables
        // NEXUS_URL = 'http://localhost:8081'
        SONAR_URL = 'http://localhost:9000'
        // DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        // REPO_NAME = 'example-repo'

        NEXUS_URL = 'http://localhost:8081/repository/maven-releases/'
        NEXUS_CREDENTIALS_ID = 'nexusRepo'


    }
    




    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NasriMedAmine/ProjetDevops.git'
            }
        }

        stage('Maven Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        
        stage('Maven compile') {
            steps {
                sh 'mvn compile'
            }
        }


       
        stage('SonarQube Analysis') {
            environment {
                // Specify SonarQube credentials if required
                SONAR_AUTH = credentials('sonartoken')
            }
            steps {
                // Run SonarQube analysis
                sh 'mvn sonar:sonar -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_AUTH'
            }
        }
        



        stage('Maven package') {
            steps {
                sh 'mvn package -DskipTests=true'
            }
        }

        // stage('clean deploy') {
        //     steps {
        //         // Exécuter la commande Maven install
        //         sh 'mvn clean deploy -DskipTests=true'
        //     }
        // }

        // stage('Maven Deploy') {
        //     steps {
        //         sh 'mvn deploy -DskipTests=true'
        //     }
        // }

        stage('Deploy to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${NEXUS_CREDENTIALS_ID}", usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                    sh 'mvn deploy -DskipTests=true'
                }
            }
        }


        
    }

    post {
        always {
            echo 'Pipeline terminé.'
        }
    }
}
