pipeline {
    agent any

    environment {
        EKS_CLUSTER_NAME = "eksdemo"
        AWS_REGION = "us-east-1"
    }

    stages {
        stage('Build Maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package'
            }
        }
        stage('Copy Artifacts') {
            steps {
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build("vaishnavijadhav1903/petclinic:${env.BUILD_NUMBER}", "./docker")
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        customImage.push()
                    }
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AKIAXYX2M6GNUW5F7A5N', credentialsId: '534174822811', secretKeyVariable: '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']]) {
                    withKubeConfig([credentialsId: 'kubeconfig']) {
                        sh 'pwd'
                        sh 'cp -R helm/* .'
                        sh 'ls -ltrh'
                        sh "pwd"

                        script {
                            sh "kubectl config use-context ${eksdemo}"
                            sh "/usr/local/bin/helm upgrade --install petclinic-app petclinic --set image.repository=vaishnavijadhav1903/petclinic --set image.tag=${BUILD_NUMBER}"
                        }
                    }
                }
            }
        }
    }
}
        


    
     