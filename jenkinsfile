pipeline{
	agent any
	tools {
		maven "maven3.8.6"
	}
	stages {
		stage('1GetCode'){
			steps{
			sh "echo 'cloning the lastest application version'"
			git branch: 'development', credentialsId: 'github_credentials', url: 'https://github.com/grin10/visa-web-app'
			}
		}
  		stage('2Test+Build'){
  			steps{
  			sh "echo 'running JUNIT-test-cases'"
  			sh "echo 'test must passed to create artifacts'"
  			sh "mvn clean package"
  			}
  		}
  		stage('3CodeQuality'){
  			steps{
  			sh "echo 'performing CodeQualityAnalysis'"
  			sh "mvn sonar:sonar"
  			}
  		}
  		stage('4uploadNexus'){
  			steps{
  			sh "mvn deploy"
  			}
  		}
  		stage('6deploy2prod'){
  		steps{
  			deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://172.31.3.136:8080/')], contextPath: null, war: 'target/*war'
  			}
  		}
  		/*
  		stage('7approval'){}
  		stage('8deploy2Prod'){}
  		stage('9Notification'){}
  		*/
	}
	/*
	options {}
	*/
	post {
		/*
		always{}
		*/
		
		success{
		emailext body: '''Hey guys!
		good job build and deployment successful

		thanks 
		Landmark''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'paypal-eam@gmail.com'
		}
		
		failure{
		emailext body: '''Hey guys!
		Build failed. please resolve issues

		thanks 
		Landmark''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'paypal-eam@gmail.com'
		}		
	}
}
