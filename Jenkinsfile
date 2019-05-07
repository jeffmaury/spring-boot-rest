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
					releaseToGitHub('jeffmaury' , 'spring-boot-rest', ${TOKEN}, 'v0.0.1', "")
				}
			}
		}
	}
}

@NonCPS
def releaseToGitHub(owner, repo, token, tag, artifact) {
	def http = new HTTPBuilder('https://api.github.com/repos/${owner}/${repo}/releases')
	http.handler.failure = {}
	http.get(path: '/tags/${tag}', contentType: JSON) { resp, json ->
		println resp.status;
	}
}
