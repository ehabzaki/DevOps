pipeline {
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
                echo 'Testing..'
                sh "docker exec -i devopschallengemaster2_web_1  python3 tests/test.py"
                sh "docker-compose down -v"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

