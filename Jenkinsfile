// //Scripted pipeline

// node {
    
//                 echo "Build"
//                 echo "Test"
//                 echo "Integration"
    

// }

//Declarative pipeline
pipeline{
	agent any
	stages{
		stage ('Build'){
			steps{
				echo "Build"
			}
		}
		stage ('Test'){
			steps{
                echo "Test"
			}
		}
		stage ('Integration Test'){
			steps{
                echo "Integration"
			}
		}
	}
}