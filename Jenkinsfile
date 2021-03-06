pipeline {
    agent {label 'slave'}
    stages 
    {
     stage('SCM'){
      steps{
       echo "checking out the repo"
       git 'https://github.com/sathish4mailing/superserve_webapp.git'
      }
     }
     
     stage('Build') {
      steps{
         sh "./gradlew build"
         }
      }
      
      stage('Quality Check'){
          steps{
              script{
                  def sonar = tool 'sonarscanner';
                  withSonarQubeEnv('Sonarqube') {
                  sh "${sonar}/bin/sonar-scanner"
                  } 
              }
          }
          
      }
     
     stage('Unit Test') {
      steps{
         sh "./gradlew test"
         }
     }
     
     stage('Docker Build')
     {
      steps{
         sh "sudo docker build -t superserve ."
         }
     }
     
     stage('Docker Deploy')
     {
      steps{
         sh "sudo docker run -itd -p 8888:8888 --name webapp superserve"
         }
     }
     
     stage('UAT Test')
     {
      steps{
         sh "sudo chmod +x geckodriver"
         sh "sudo chmod +x seleniumtest.jar"
         sh "java -jar seleniumtest.jar"
         }
     }
     
     stage('Destroy Container')
     {
      steps{
         sh "sudo docker stop webapp"
         sh "sudo docker rm webapp"
         }
     }
     stage('Docker push'){
     
       steps{
         sh "sudo docker login -u sathish4mailing -p $paswd"
         sh "sudo docker tag superserve sathish4mailing/superserve:v2"
         sh "sudo docker push sathish4mailing/superserve:v2"
         }
      }
     stage('Cleanup')
     {
      steps{
         echo "cleaning up he workspace"
         cleanWs()
         }
      }     
   }
}
