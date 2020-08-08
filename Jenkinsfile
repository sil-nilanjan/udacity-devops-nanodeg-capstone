pipeline {
	agent any
	environment {
			VERSION = 'latest'
			PROJECT = 'udacitycapstone-nextcloud'
			IMAGE = "$PROJECT"
			ECRURL = "https://627513304485.dkr.ecr.eu-west-2.amazonaws.com/$PROJECT"
			ECRURI = "627513304485.dkr.ecr.eu-west-2.amazonaws.com/$PROJECT"
			ECRCRED = 'ecr:eu-west-2:ecruser'
	}
	stages {
		stage("Lint Dockerfile") {
			steps {
				sh "docker run --rm -i hadolint/hadolint:v1.17.5 < Dockerfile"
			}
		}
		stage('Build Prep') {
			steps {
				script {
					// calculate GIT lastest commit short-hash
					gitCommitHash = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
					shortCommitHash = gitCommitHash.take(7)
					// calculate a sample version tag
					VERSION = shortCommitHash
					// set the build display name
					currentBuild.displayName = "#${BUILD_ID}-${VERSION}"
				}
			}
		}
		stage('Dockerizing') {
		  	steps {
				script {
					// Build the docker image using a Dockerfile
					docker.build("$IMAGE")
				}
      		}
		}
		stage('Docker Push') {
			steps {
				sh "docker login -u AWS -p eyJwYXlsb2FkIjoiRFRBc0tObTNEcEQ3czBTalJpaXlWbGdxR2ttMW05Tmg2dXhGaTYvclZ5bXpSSjl0dTRPZngydWdYL3NjMC9GcFA4a2tneE5tUUtqbHJubXVJTXJnbzQ4czV1RVB6ZDJaYzhGT1B6VDZTb3hyaUpsZlQzWHE4eUYyRGE4cDc5VnZLME12bjFhdGFSWkt1M2RqK09aUjVSemNBU3pMSC9SV3NJYythcVVLdld1TzYreXNVU3U0eHQ3QURyaExIVlNKVTZWaCtBZ201M3FsdkMyWnlGVldRWlR3MlhKRzJ1NTFyTVRTbWxRLzRRYVg0VXJlZXRXUXBFb2JuM1lNeE82YndDQWVFNTVXUWtaMGkxNlM4TG9BRG9RQVBoZ0NycmRWQjFDV29iQzZjbVhzNUg3S0l3TDlIdzRUNWdWTzhPWk1jM3k4dm9maEU2cy8vQnppanNwbjJTckpnNTBEUGMrSW84aXRLeTJRbWl5RThoMlE3SG5jeHlHVWIxV3hhNGdoQjA3RlhBVnpTYXAwQlVESVN3S1h6ZDAzSEc3ZVQrRmNPaXBuTG5DZ2NzSFQwTWVVc3hPWDRncUpKOVBKRlJVeUFKNnBsMm9VRWwrSUZlSFB4WlFYLzI0VXB2N05hOVo4Y25Xbm5qOC91Vlc4WDhuektsRk9wWWUySjlybTZHd3RYS3lFT0lId3JmU3NBUDMwODEvLy80Y1Nnb25nWUk4SjhOTlJsQUNMMU5rSlUrTEVxRXV1eEJ6SVlieEhTRXVsRXNiU1Q4QW5ZNmNaMGRSVE9NamRQUmVYaWJjRDY2OFV5dDYrUHI0M1BBWXlsNERlUDdWTE9Oc0NFeWNwZlM0UVozNVNOaDI5a3dUczd0TTd1SkVxdzNGOVVXNjJsWG1HaEdhbVAzNUMxMFpqeVQyRExaKzdQZWcvWDJlazZ1TFVKRE05MFM3Vk4rWlhoMzV1cWE3YXo3aWd5cUhEQktySERwN3pqVVEzNStzZjRHeUFIOWJPZ1kxc3hIa0tlWVltZ2FreGE1N0gvcUQ0MzZOTmFiaVRSaHZxb3h5QjBOekJuV2pUR1M2NldpeHp6MkVhRDREaVJLN2o2VGZkdVZBa2lMVEtwZC9Vem5ZeTB1REtFSHptS3JFQmtldmNqSTl1YU5hU1JPVHdhYXhDQ0tweHJRQVZ1TWJtM2tsajFJT2dyblU3YWlpL1Y5dTd5Sm10RUU3bXU3WUVIcVROY1hOOURqd2Q3aUptOWxxWmVnWXBKOHFSaEJGTmhhMUNZcEdiQnArVnhSY0NDZmFUbE1oTWtjR2d5Q25yeE5pWEI0V1psMzA2RjlibUd3cmhYN2VtVFYvd0JvUVloajBWZmdZTU14dzVsS3pkaE9MYlg3MUd6eWgzQmU4NWJBZW55NEJoTEZaSHRJN3NEL01kcXFGbFRuMUJCSTVjRHZnUWJNeEdOZVFxaDZiK3NFN2VWUzk5eWlCZlVLL0tKbUZGN0Z1SU9rQ3A1blJhaXJ2K0J2TXl6TnRsQmdJUGpCcXBmSHZlcmd2OGlrdnBpVFRYUFpINVVldjk2eWZRIiwiZGF0YWtleSI6IkFRRUJBSGhORHpXVm5ZR3lsSzN0VWFsSEFOcG1yblF6YTV3NFc0Y0hZTVU4MnRiTkl3QUFBSDR3ZkFZSktvWklodmNOQVFjR29HOHdiUUlCQURCb0Jna3Foa2lHOXcwQkJ3RXdIZ1lKWUlaSUFXVURCQUV1TUJFRURJYzA4b0k2eG41ZFpqV0c0UUlCRUlBN2ROdmlTWDBlVEs4RTllY1JKa2hVYkNLek5VYkpiZHNoYTQ2UUZ0WWhRYW5PWmR5a0VLdEZ1L3NOMWN4QTJSRXE2dU51dmZ0QWxsb1RvMWs9IiwidmVyc2lvbiI6IjIiLCJ0eXBlIjoiREFUQV9LRVkiLCJleHBpcmF0aW9uIjoxNTk2OTMyMjY4fQ== https://627513304485.dkr.ecr.eu-west-2.amazonaws.com"
				sh "docker tag udacitycapstone-nextcloud 627513304485.dkr.ecr.eu-west-2.amazonaws.com/udacitycapstone-nextcloud"
				sh "docker push 627513304485.dkr.ecr.eu-west-2.amazonaws.com/udacitycapstone-nextcloud"
			}
		}
		}
		stage('K8S Deploy') {
			steps {
					withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}",
                                                 "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}",
                                                 "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
						sh "aws eks --region eu-west-2 update-kubeconfig --name UdacityDevOpsNanodegreeCapStone-Cluster"
						// Configure deployment
						sh "kubectl apply -f k8s/deployment.yml"
						// Configure service for loadbalancing
						sh "kubectl apply -f k8s/service.yml"
						// Set created image to do a rolling update
						sh "kubectl set image deployments/$PROJECT $PROJECT=$ECRURI:latest"
					}
			}
		}
  	}
	post {
		always {
		    // make sure that the Docker image is removed
		    sh "docker rmi $IMAGE | true"
		}
	}
}
