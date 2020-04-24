pipeline {
  agent { label 'slave'}
   stages 
   {
     stage('Clone'){
      steps{
       echo "checking out the repo"
       git 'https://github.com/AbdulItsMe/superserve_webapp.git'
       }
     }
     stage('Build')
     {
      steps{
         sh "./gradlew.build"
         }
     }
     stage('Docker Build')
     {
      steps{
         sh "docker build -t superserve ."
         }
     }
     stage('Docker Deploy')
     {
      steps{
         sh "docker run -itd -p 8888:8888 --name webapp superserve"
         }
     }
     stage('UAT Test')
     {
      steps{
         sh "sudo chmod +x geckodriver"
         sh "java -jar seleniumtest.jar"
         }
     }
     stage('Destroy Container')
     {
      steps{
         sh "docker stop webapp"
         sh "docker rm webapp"
         }
     }
     stage('Docker push'){
     {
       steps{
         sh "docker tag superserve abdulitsme/superserve:v1"
         sh "docker push abdulitsme/superserve:v1"
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
     
