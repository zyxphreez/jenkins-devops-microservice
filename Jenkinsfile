// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }

pipeline {
	//agent any
	// agent { docker { image 'maven:3.6.3' }}
	agent { nodejs { image 'node:13.8' }}
	stages {
		stage ('Build') {
			steps {
				//sh 'mvn --version'
				sh 'node --version'
				echo 'Build'
			}
		}
		stage ('Test') {
			steps {
				echo 'Test'
			}
		}
		stage ('IntegrationTest'){
			steps {
				echo 'Integration test'
			}
		}
	}
	post {
		always {
			echo 'Im running always'
		}
		success	 {
			echo 'Im running on success'
		}
		failure {
			echo 'Im running when the pipeline fail'
		}
	}
}