#!/usr/bin/env groovy
pipeline {
	tools {
		maven 'maven_3.6.0'
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
