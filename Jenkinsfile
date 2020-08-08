pipeline {
	agent any
	environment {
			VERSION = 'latest'
			PROJECT = 'udacitycapstone-nextcloud'
			IMAGE = "$PROJECT"
			ECRURL = "https://627513304485.dkr.ecr.us-west-2.amazonaws.com/$PROJECT"
			ECRURI = "627513304485.dkr.ecr.us-west-2.amazonaws.com/$PROJECT"
			ECRCRED = 'AWS/eyJwYXlsb2FkIjoic21sbzROSGFwOTJIaTJnREQrenRGNUtWcE4rM3dhK1JGZ0hjZitwTDBMZ0tmdzZsQk44dDQyaWVCT0lLQmZKMC9qb1oxcHNEcmM4SXFZTVpLaENrRmMwTm1jaUM3Zm9QZDE4TVBucmZrNFhCZGhDQjIxOUlIREc4YVpIc1FnWTBadkRwUnpzb29uMzZmUnFmbXIvTG8yY1pHbWdXbkpJTlFGcVZ6SlBUTmhRa1dNaEt0VHFYS1p6aDc3VTY1a2RDandES3krWEExenFQazQ5TG5mNUNFZlJZMGw3RnNJd3hrU1N5bVUrMkd2RmdIK1FmN09jZURPNWc4Z1BiYS9lbDVTSS9xU0ova1ZORDNYK1Bkcmt0OHJQVk8zS1hjWWwybjQ1MEZtcGR0VHliRlNwaFRSZEIwWTdSam42OEhZcVVRWlN3Um9iS1NUYlBYRDYzbk5wOURxemZTSDZvTU83NXRwaUNVNS9EV1UwaGdpSTl6YndaSWVnNkk2bHVCVHMzME5nTlJsSzk4NGI4czRCUlVIWWxwTTdYVVYySkp5U1F3T1VhbGFjZVRodk0vajdCWkVLYzVsaWQ2UmM2UFg3OTU0QXNIcVpGajZkVnNYampVdTYyM0YwQTRBeldHd205RnVyNGkvOEJ1eDlCQXFMMVBvNDlvVzROem5aK0cvdkxoUDhqajRxK1FkWTVXcWpZMkJ4UnRhOUtEc2lzVVZIc0N5ckdGNisweGZzSVpCMWNWSlRGcE5rUmlPY0JwM0g3akVmVDFmOE1XMWNYbWRJLzM0SEl6Y0RyYWZocHNyNTIvbldVaVc3VTFOTjRUWlI4MDFLSXFlUmx4bWI1Nm1VL2pmS3lEQk1rcFcvdFBxSHI4OVBvQ2l2UDdhY0UxV2FwVm95K3ZzNEJjQkk4M0RhdlZKZTIrUS85MnNrY3ZLRmdNTmpLTStMQ09heVVqTVQveVNxQ1RYQ1V1RGR4cGJtamhoVU5mbEU4dXN2emVScXZ6VkxpbVl5UFhnalV4UEUxTytURG1OSS9Hd25FaHdRNDJlalhrM2tYVEdrWDRtV2JFMmYybyszdmtybC84U2JCMnExMEcxWjZlanh6eGZrclpCWlhkMk0vTVcvZkNoKzl0MDJ4WFVaYjk3anlST2V0bk5tYi9ybmt4VnJySGNqN20va0l6Wlh6cCtuT2paaGdQTjU0eVNRbjRqaVNNY2J6UklXaEV6ckpxbzI0c2c5YTFKeEdQTkphbWtKd3N0SElvVWRZZ1QvT0M4Nm5QK1k0L0NGZGxFQW1qbXV4SWd2Y1daYlg4R3NOQThOSHFHTEp0U0VZdmxPTlRLdGVmTzJRaTg5QjRkUU85NnhQNWhRNUQ5dnNzZXcwUXUxd3VyYnhSeGMwQjZWcWVTUU5BZmdIdzFnUlltMlFQdm1IM0JsVHRid3ZHaHJUNlZoRE1aVzZNN1d0MFdBOWgzSUpweWdwU1YwbnV4ZDY1MmZyR1JzQ2NlK2tFT1FlaDZIdHh6QUpuZ2FUZWpJRFpMZk9TS3R0Zmt0OUU2U1dDRHRWOTNBc0NyaDNJQkxiIiwiZGF0YWtleSI6IkFRRUJBSGhORHpXVm5ZR3lsSzN0VWFsSEFOcG1yblF6YTV3NFc0Y0hZTVU4MnRiTkl3QUFBSDR3ZkFZSktvWklodmNOQVFjR29HOHdiUUlCQURCb0Jna3Foa2lHOXcwQkJ3RXdIZ1lKWUlaSUFXVURCQUV1TUJFRURCbnRjN2NGdlpDMWVOdksxQUlCRUlBNzFWbUNLaGtuRzkzYWhKUmpCMXhmbE9VU2prSFFMUElYVTlnWlNCaHIvbGt0VXdIcHd2ak9HaUxNc2pRTitGbG5nNDd4UUkraU5rOXpnT0k9IiwidmVyc2lvbiI6IjIiLCJ0eXBlIjoiREFUQV9LRVkiLCJleHBpcmF0aW9uIjoxNTk2ODk3NDI0fQ=='
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
				script {
					// Push the Docker image to ECR
					docker.withRegistry(ECRURL, ECRCRED) {
						docker.image(IMAGE).push("latest")
						docker.image(IMAGE).push(VERSION)
					}
				}
			}
		}
		stage('K8S Deploy') {
			steps {
					withAWS(credentials: 'jenkins', region: 'us-west-2') {
						sh "aws eks --region us-west-2 update-kubeconfig --name UdacityDevOpsNanodegreeCapStone-Cluster"
						// Configure deployment
						sh "kubectl apply -f k8s/deployment.yml"
						// Configure service for loadbalancing
						sh "kubectl apply -f k8s/service.yml"
						// Set created image to do a rolling update
						sh "kubectl set image deployments/$PROJECT $PROJECT=$ECRURI:$VERSION"
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
