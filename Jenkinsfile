pipeline {
	agent any
	options {
		skipDefaultCheckout()
	}
	stages {
		stage ('Checkout') {
			steps {
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
							bat '"C:\\Program Files\\dotnet\\dotnet.exe" clean -c release -o /app' 
							bat '"C:\\Program Files\\dotnet\\dotnet.exe" publish -c release -o /app --no-restore' 
						}
					}
				}
		}
		stage('Deploy') {
			steps {
					azureWebAppPublish azureCredentialsId: params.AZURE_CRED_ID,
					resourceGroup: params.RES_GROUP, appName: params.WEB_APP, sourceDirectory: "C:\\app"
				}
		}
	}
}

