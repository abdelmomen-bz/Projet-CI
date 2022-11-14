
pipeline {
          agent any
          stages{
            stage('Checkout GIT'){
                steps{
                    echo 'Pulling...';
                    git branch: 'main',
                    url : 'https://github.com/abdelmomen-bz/Projet-CI.git';
                }

            }
            stage('MVN CLEAN'){
            steps{
                echo 'Pulling...';
                sh 'mvn clean'
                }
            }
             stage('MVN COMPILE'){
                steps{
                sh 'mvn compile'
                }
             }
             stage('MVN PACKAGE'){
                steps{
                sh 'mvn package'
                }
             }
             stage('MVN TEST'){
                steps{
                 sh 'mvn test'
                 }
             }

              stage('MVN SONARQUBE '){
                 steps{
                    sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=esprit'
                 }
              }
              stage("nexus deploy"){
                 steps{
                  nexusArtifactUploader artifacts: [[artifactId: 'tpAchatProject', classifier: '', file: '/var/lib/jenkins/workspace/ProjetCI/target/docker-spring-boot.jar', type: 'jar']], credentialsId: 'nexus-snapshots', groupId: 'com.esprit.examen', nexusUrl: '192.168.10.141:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexus-snapshots', version: '2.2.4'
                 }
              }
              stage('Build Docker Image') {
                 steps {
                 sh 'docker build -t abdelmomen/projetspring:2.2.4 .'
                 }
              }

              stage('Push Docker Image') {
                   steps {
                   withCredentials([string(credentialsId: 'DockerhubPWS', variable: 'DockerhubPWS')]) {
                       sh "docker login -u abdelmomen -p ${DockerhubPWS}"
                   }
                     sh 'docker push abdelmomen/projetspring:2.2.4'
                   }
              }
              stage('DOCKER COMPOSE') {
                   steps {
                      sh 'docker-compose up -d --build'
                   }
              }
          }
       }








