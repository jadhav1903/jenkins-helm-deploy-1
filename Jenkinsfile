pipeline {
    agent any
    stages {
        stage ('Build maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package' 
            }
        }
        stage ('Copy Artifacts') {
            steps{
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }
        stage ('Unit test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build("vaishnavijadhav1903/petclinic:${env.BUILD_NUMBER}", "./docker")
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    customImage.push()
                }
            }
        }
    }
}