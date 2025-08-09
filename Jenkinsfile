pipeline{
    agent any 
    tools{
        maven 'maven'
        jdk 'jdk'
    }
    environment{
        DOCKER_PASS = credentials('dockerhub-pass')
        DOCKER_NAME = 'muhammedhamedelgaml'
    }
    stages{
        stage("Dependancy check"){
            steps{
                sh "mvn dependency-check:check"
                dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
            }
        }
        stage("build app"){
            steps{
                sh "mvn package install"
            }
        }
        stage("archive app"){
            steps{
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }
        stage("docker build"){
            steps{
                sh "docker build -t ${DOCKER_NAME}/iti-java:v${BUILD_NUMBER} ."
                sh "docker images"
            }
        }
        stage("docker push"){
            steps{
                sh "docker login -u ${DOCKER_NAME} -p ${DOCKER_PASS}"
                sh "docker push hassaneid/iti-java:v${BUILD_NUMBER}"
            }
        }
    }
}
