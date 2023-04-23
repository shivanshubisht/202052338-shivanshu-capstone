pipeline {
    agent any

     environment{
       registryCredential = 'ecr:ap-south-1:shivanshu'
       appRegistry = "434954100083.dkr.ecr.ap-south-1.amazonaws.com/202052338-shivanshu-capstone"
       capstoneRegistry = "https://434954100083.dkr.ecr.ap-south-1.amazonaws.com"
       cluster = "202052338-capstone-shivanshu"
       service = "capstoneapp"
   }

    stages {
        stage('Clone Website') {
            steps {
                git url:'https://github.com/shivanshubisht/202052338-shivanshu-capstone'
            }
        }

       stage("Docker Build Image"){
           steps{
             script{
                dockerImage = docker.build(appRegistry+ ":$BUILD_NUMBER",".")
             }
           }
      }

       stage('Test Website') {
            steps {
                // Run tests on the website
                sh 'echo "Running tests on the website"'
            }
       }

       stage("Upload App Image"){
         steps{
            script{
                docker.withRegistry(capstoneRegistry, registryCredential){
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                }
            }
         }
      }

      stage('Deploy to ecs'){
         when {
                branch "master"
         }
         steps{
            withAWS(credentials: 'shivanshu', region: 'ap-south-1'){
                sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
            }
         }
      }

    }
}
