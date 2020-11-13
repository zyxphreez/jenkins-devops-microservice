// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }

pipeline {
	agent any
	// agent { docker { image 'maven:3.6.3' }}
	// agent { docker { image 'node:13.8' }}
	environment {
		dockerHome = tool 'MyDocker'
		MavenHome = tool 'MyMaven'
		PATH = "$dockerHome/bin:$MavenHome/bin:$PATH"
	}
	stages {
		stage ('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				// sh 'node --version'
				echo "PATH $PATH"
				echo "BUIL_ID - $env.BUILD_ID"
				echo "BUIL_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_DISPLAY_NAME - $env.BUILD_DISPLAY_NAME"
				echo "BUILD_URL - $env.BUILD_URL"
				echo "BUILD_TAG - $env.BUILD_TAG" 
			}
		}
		stage ('Compile') {
			steps {
				sh 'mvn clean compile'
			}
		}
		stage ('Test'){
			steps {
				sh 'mvn test'
			}
		}
		stage ('Integration Test'){
			steps {
				sh 'mvn failsafe:integration-test failsafe:verify'
			}
		}
		stage ('Package'){
			steps {
				sh 'mvn package -DskipTests'
			}
		}
		stage ('Build Docker Image'){
			steps {
				//docker build -t zyxphreez/currency-exchange:$env.BUILD_TAG
				script {
					dockerImage = docker.build("zyxphreez/currency-exchange:${env.BUILD_TAG}")
				}
			}
		}
		stage ('Push to DockerHub'){
			script {
				docker.withRegistry('', 'dockerhub')
				{
					dockerImage.push();
					dockerImage.push('latest');
				}
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