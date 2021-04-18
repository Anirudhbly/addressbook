pipeline {
        agent {
            label 'master'
        }
        tools {
            maven 'My_Maven'
            jdk 'My_JAVA'
        }
    stages {
        
        stage ('Checkout the code') {
            steps{
                git branch: 'main', url: 'https://github.com/Anirudhbly/spring-petclinic.git'
            }
        }
       stage ('Parallel block') {
       parallel {           
        stage ('Code validate') {
            steps{
                catchError (buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                sh """
                mvn validate
                """
            }
            }
        }
        stage ('Code Compile') {
            steps{
                catchError (buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                sh """
                mvn compile
                """
        }
        }
       }
      }
        stage ('JUNIT test') {
            steps{
                sh """
                mvn test
                """
            }
        }
        stage ('Packaging') {
            steps {
                sh """
                mvn package
                """
            }
        }
      }  
    post {
        always {
            junit 'target/surefire-reports/**/*.xml' /* pulling the test results */
        } 
    }     
}
