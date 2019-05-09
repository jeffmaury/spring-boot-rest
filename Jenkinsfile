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
					releaseToGitHub('jeffmaury' , 'spring-boot-rest', '${TOKEN}', 'v0.0.1', findFiles(glob: '**/*.jar')[0].path)
				}
			}
		}
	}
}

 
@NonCPS
def releaseToGitHub(owner, repo, token, tag, artifact) {
	sh """
	releaseId=\$(curl "https://api.github.com/repos/${owner}/${repo}/releases/tags/${tag}" | sed -n -e 's/"id":\\ \\([0-9]\\+\\),/\\1/p' | head -n 1 | sed 's/[[:blank:]]//g')
	if [ -z "\$releaseId" ]
	then
		releaseId=\$(curl -XPOST -H "Authorization:token ${token}" --data "{\\"tag_name\\": \\"${tag}\\"}" "https://api.github.com/repos/${owner}/${repo}/releases"| sed -n -e 's/"id":\\ \\([0-9]\\+\\),/\\1/p' | head -n 1 | sed 's/[[:blank:]]//g')
	fi
	if [ -z "\$releaseId" ]
	then
		echo "No release, exiting"
	else
	    artifactName=\$(basename ${artifact})
		echo "\$artifactName"
		artifactId=\$(curl -XPOST -H "Authorization:token ${token}" --data-binary "@${artifact}" "https://api.github.com/repos/${owner}/${repo}/releases/\$releaseId/assets?name=\$artifactName"| sed -n -e 's/"id":\\ \\([0-9]\\+\\),/\\1/p' | head -n 1 | sed 's/[[:blank:]]//g')
		if [ -z "\$artifactId" ]
		then
			echo "No artifact, exiting"
		else
			echo "Artifact id = \$artifactId"
		fi
	fi
	"""
}
