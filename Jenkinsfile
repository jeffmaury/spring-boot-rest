#!/usr/bin/env groovy
pipeline {
	tools {
		maven 'maven_3.6.0'
	}
	
	stages {
		stage('Checkout repo') {
			deleteDir()
			git url: 'https://github.com/jeffmaury/spring-boot-rest'
		}

		stage('Build') {
			sh 'mvn package -B'
		}
	}
}
