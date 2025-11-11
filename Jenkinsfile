pipeline {
    agent any

    tools {
        // Install the Maven version configured as "mvn" and add it to the path.
        maven "mvn"
    }

    stages {
        stage('Git checout') {
            steps{
                // Get some code from a GitHub repository
                git 'https://github.com/sumeetverma07/simple-java-maven-app-may-24.git'
            }

           // post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
              //  success {
               //     junit '**/target/surefire-reports/TEST-*.xml'
                 //   archiveArtifacts 'target/*.jar'
              //  }
          //  }
        }
          stage('Build'){
            steps{
                sh 'mvn clean package'
            }
          }
           stage('Docker Build') {
            steps {
                sh 'docker build -t java .'
        
            }
        }  

        stage('Docker push to the dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'verma00', passwordVariable: 'verma00')]) {
 {
                sh 'docker login -u verma00 -p ${Pass}'
                sh 'docker tag java verma00/devops1:latest '
                sh 'docker push verma00/devops1:latest'
                sh 'docker logout'
                }           
                
            }
        }  
          stage('Approval'){
            steps{
                input 'Approve Test to the script'
            }
          }
          stage ('Test'){
            steps {
                sh 'mvn test'
            }
            post {
                success{
                    junit '**/target/surefire-reports/*.xml'
                    archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false, onlyIfSuccessful: true
                }
            }
          }

          stage('Deploy'){
            steps{
                sh 'echo Deployed'
            }
          }
        }
    }
