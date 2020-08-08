pipeline {
	agent any
	environment {
			VERSION = 'latest'
			PROJECT = 'udacitycapstone-nextcloud'
			IMAGE = "$PROJECT"
			ECRURL = "https://627513304485.dkr.ecr.us-west-2.amazonaws.com/$PROJECT"
			ECRURI = "627513304485.dkr.ecr.us-west-2.amazonaws.com/$PROJECT"
			ECRCRED = 'ecr:us-west-2:ecruser'
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
				sh "docker login -u AWS -p eyJwYXlsb2FkIjoiL2w5RkRDZHdSclZpS0J3OHFPSzJJVFVLdk5FR25IVzE5TEtteWNQTGh5SFhqV1cvdnlxaU5vUXQyU0k4eWR1YzlQbnljR3pXNWVVZ0hXRVhiMTRPa3VTc2FQOU54KzR2WjRVYzZmZ3NTZ215WEJmVDgxbnRKRVRCS0FyemdMUGJha1dGZkRvTVZTU3g0a1cvSCtlT0hoYkhZRFQya28rS01mTUZKSUZHdVI0V0svNmx2elNFTXk1OVVLZERJNUpCcC9YS1dyc3ExTU5WWUNLRXBiRGo1alhmN2c0aGlBWnpJQ2VaNkNNTklWcklVcVNIYU9rdzhuWTlUR0oweHl6NHVvTE80M1EyNEg2eHlybUxJSmZVNlFSbjB0am9UcUhOZWs5UWk1ekxZUjQwREY3bWlLdHNMNG5hUDFzSjkwNGp1N09SZHBtSUtaU1FhTVVaa1YrZnBWS0RtYmppaFJYbWJUSFZOYVJDanpBNlJIa0VtRWhEUHlzRmkrc3JXM3RoY0xQY3lNb0xzTFZBLzZRNjZ0U3FPNEhwZnJGak9ENHBJSldGMW9JUi9TejFoT3k1UVY2bGpRT3BBYUxrTjE5UDVYN3BVU2JQK2hjR2NXZ1JkVnR4N3FBVWlBY2o0NEhvVmdFR2ZTRCttS2E2YW1LcUE4OFI2cnlxYjd6U1doRUZkZTNqUGhlaXM4WkhESUllN2pGZnpKZFo4SHpSejZXMC9jTFAwUkZONDRaUVVER3FBV3loNWxqNm1NVXNDZGJPa3E0TXhJM3o4enhabmpXbENIVXh4bVRhY0VqT0ROaW1LdmMrNjZ0eENzRU93VDdhVWRWcE92VmJQdms5aG9Vd0Q5WGVrL1RWRjdtVnBZL2FtbkZsZ3VoNWNlWjR0eHlBT1UyUWp2WXFaU21IRTBKRDhTcjdSS3gxK0x2R1JQODlXU2JOTFdwQkhaWXJrWTl2TTRwMHhERjIydDFiNTNPSUpyT25lZitzRnl1Ykg1Z2Z2RDZCOHVDTzhlLzdpbkptbjR5R1BQWUh5R3hvMDV6ZWZ2K3lvaElaMXFISWtNM0JIZzdpblJlMjJwQjVoaGd2ZCtQck5IcDVhWEpQanRnWjhLVE9zeXJvVkVMS1BFR3BoVGpHNjRabVNMc2JBRzR1d3B4T1I1M0g2MnkxSnNZanBFTTFOUlpVL3R0TU9BRjJxbE9jcldaSnFYUnlleGl5ZE1JRzQ4dkFxcHgyWHJHbnpQOFFOUThkSHFheW1USkl3aUI4bWo3VlhxTHRmRkF1SHl6aHdZRjJCcGY3MzZtS0lmeTVUa3RWVTdYN3k4eXp4dWZRdzhNc0Z4ODlTa1BlL3Zsb095NnY5Uis5Qlh5NjF5RS94U3dSYU0ySjBibHNGVk8wREZuMy9aVzFIMEtYNXFYeHc0OVFlbzEvNEgybm85NDRSS1V5cXdKTFBtUnBiUVhPc0hOY0JqOVI1SDF4WlNXOGF5R1N5MlNXeGRnbXRTbG1BbFJUdW8yUmQyR0hMN0lxNytXZ29oWXh5KzROMlRtVDd4U0Y1K1dMT3R6ZlZPeFlHWjNrIiwiZGF0YWtleSI6IkFRRUJBSGo2bGM0WElKdy83bG4wSGMwMERNZWs2R0V4SENiWTRSSXBUTUNJNThJblV3QUFBSDR3ZkFZSktvWklodmNOQVFjR29HOHdiUUlCQURCb0Jna3Foa2lHOXcwQkJ3RXdIZ1lKWUlaSUFXVURCQUV1TUJFRURLd3RoQ1FUcDhTNEFSRXRTd0lCRUlBN29vY0dUcjlBSGVkWXgzeklBOHc5clpmNTNFZ1FUZ0Fvb040TXc4T01IRDQrd29sRFFxcWRMTlY0cnIwUllFMGpUL1YvTkQ4dHkzRnl0bXc9IiwidmVyc2lvbiI6IjIiLCJ0eXBlIjoiREFUQV9LRVkiLCJleHBpcmF0aW9uIjoxNTk2OTAwOTU1fQ== https://627513304485.dkr.ecr.us-west-2.amazonaws.com"
				sh "docker tag udacitycapstone-nextcloud 627513304485.dkr.ecr.us-west-2.amazonaws.com/udacitycapstone-nextcloud"
				sh "docker push 627513304485.dkr.ecr.us-west-2.amazonaws.com/udacitycapstone-nextcloud"
			}
		}
		stage('K8S Deploy') {
			steps {
					withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}",
                                                 "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}",
                                                 "AWS_DEFAULT_REGION=us-west-2"]) {
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
