// //Scripted pipeline

// node {
    
//                 echo "Build"
//                 echo "Test"
//                 echo "Integration"
    

// }

//Declarative pipeline
pipeline{
	agent any
	// agent { docker { image  'maven:3.6.3'} } //perrmission denied error 
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage ('Checkout'){
			steps{
				sh 'mvn --version'
				// sh  'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"

			}
		}
		stage('Compile'){
			steps{
				sh "mvn clean compile"
			}

		}
		stage ('Test'){
			steps{
                sh "mvn test"
			}
		}
		stage ('Integration Test'){
			steps{
                sh "mv failsafe:Integration-test failsafe:verify"
			}		
		}
		stage ('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}
		stage ('Build Docker Image') {
			steps{
				// "docker build -t kirik/currency-exchange:$env.BUILD_TAG"
				script {
					docker.build("kirik/currency-exchange:$(env.BUILD_TAG)")
				}
				
			}
		}
		stage ('Push Docker Image') { //configure your docker hub credentials in jenkinns first ( credentials - jenkins - global_credentials )
			steps{
				script{
					docker.withRegistry ('','dockerhub') { //Credential_Id given in credentials section
						dockerImage.push();
						dockerImage.push('latest'); 

					}
				}

			}
		}
	}
	post{
		always{
			echo 'Im awesome. I run always'
		}
		success{
			echo 'I run when you are successful'
		}
		failure{
			echo 'I run when you  fail'
		}
	}
}