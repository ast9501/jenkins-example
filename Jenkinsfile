pipeline{
    agent any
    stages {
       stage('build example image'){
           steps{
               sh 'docker build . -t jenkins-build:v1 '
           }
       }
       stage('Finished'){
           steps{
               sh 'echo "Build finished..."'
           } 
       }
    }
}
