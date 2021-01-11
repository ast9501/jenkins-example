pipeline{
    agent any
    stages {
       stage('build'){
           steps{
               sh 'docker build -t jenkins-demo:v1 .'
           } 
       }
    }
}
