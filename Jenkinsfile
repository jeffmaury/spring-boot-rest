#!/usr/bin/env groovy
pipeline {
	agent any
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
		
		stage('Publish') {
			steps {
				withCredentials([[$class: 'StringBinding', credentialsId: 'Github', variable: 'TOKEN']]) {
					releaseToGitHub('jeffmaury' , 'spring-boot-rest', '${TOKEN}', 'v0.0.1', "")
				}
			}
		}
	}
}

 
@NonCPS
def releaseToGitHub(owner, repo, token, tag, artifact) {
	sh """
	release=\$(curl "https://api.github.com/repos/${owner}/${repo}/releases/tags/${tag}" | sed -n -e 's/"id":\\ \\([0-9]\\+\\),/\\1/p' | head -n 1 | sed 's/[[:blank:]]//g')
	if [ -z "\$release ]
	then
	else
	fi
	"""
}
