pipeline {
agent any
 
options {
    skipDefaultCheckout()
}

stages {
stage ('Checkout') {
        steps{
            checkout(scm)
            stash includes: '**', name: 'source', useDefaultExcludes: false
        }
    }
stage ('Restore Packages') {     
         steps {
             deleteDir()
             unstash 'source'
             script {
                 bat '"C:\\Program Files\\dotnet\\dotnet.exe" restore "src\\dotnet-jenkins-demo.sln" '
             }             
          }
        }

stage('Build') {
     steps {
            deleteDir()
            unstash 'source'
            dir('src\\dotnet-jenkins-demo'){
                script{
                    bat '"C:\\Program Files\\dotnet\\dotnet.exe" publish -c release -o /app --no-restore' 
                }
				zip zipfile "dotnet-jenkins-demo.zip"
				zip zipfile "dotnet-jenkins-demo.zip"
            }
			
      }
   }
stage('Deploy') {
	steps {
			dir('src\\dotnet-jenkins-demo'){
			 azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
			  resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "dotnet-jenkins-demo.zip"
			}   
	   }
	}
 }
}

