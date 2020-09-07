pipeline {
  environment {
    image = "ehab123/tradebyte"

  }
    agent any
    

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "docker-compose  build"
                sh "docker-compose  up -d"

            }
        }

        stage('Test') {
            steps {
                script{
                echo 'Testing..'
                dockerImage = docker.build image + ":$BUILD_NUMBER"
                sh "docker run -i ${image}:${BUILD_NUMBER}  python3 tests/test.py"
                sh "docker-compose down -v"
               }
                
            }
        }
        stage('push image') {
            steps{
                script {
                    //dockerImage = docker.build image + ":$BUILD_NUMBER"

                    docker.withRegistry( '', "registryCredential" ) {
                    dockerImage.push()
                }
            }
        } 
    }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
		sh "docker stack deploy --compose-file tradebyte-development.yml  dev"
               sh "docker stack deploy --compose-file tradebyte-production.yml  prod"
            }
        }
    }
}

