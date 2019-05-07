#!/usr/bin/env groovy
pipeline {
	agent any
	tools {
		maven 'maven_3.6.0'
	}
	
	@NonCPS
	def releaseToGitHub(owner, repo, token, tag, artifact) {
	}
	
	stages {
		stage('Checkout repo') {
			steps {
				deleteDir()
				git url: 'https://github.com/jeffmaury/spring-boot-rest'
			}
		}

		stage('Build') {
			steps {
				sh 'mvn package -B'
			}
		}
	}
}
