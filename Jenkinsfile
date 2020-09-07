pipeline {
  environment {
    image = "ehab123/tradebyte"

  }
    agent any


    stages {
        stage('Test') {
            steps {
                script{
                echo 'Testing..'
                dockerImage = docker.build image + ":$BUILD_NUMBER-$GIT_BRANCH"
                  
                sh "docker run -i ${image}:${BUILD_NUMBER}-${GIT_BRANCH}  python3 tests/test.py"

               }

            }
        }
        stage('push image') {
            steps{
                script {
                    docker.withRegistry( '', "registryCredential" ) {
                    dockerImage.push()
                }
            }
        }
    }

        stage('staging deploy') {
            when {
                branch "development"
            }
            steps {  

                echo 'Deploying on staging'
                sh "echo BUILD_NO=${BUILD_NUMBER}-${GIT_BRANCH} >> .env"
                sh "docker stack deploy --compose-file tradebyte-development.yml  dev"
            }
        }
        stage('Production deploy') {
            when {
                branch "master"
            }
            steps {  

                echo 'Deploying on production....'
               sh "echo BUILD_NO=${BUILD_NUMBER}-${GIT_BRANCH} >> .env_prod"
               sh "docker stack deploy --compose-file tradebyte-production.yml  prod"
            }
        }
    }
}

