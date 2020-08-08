pipeline {
	agent any
	environment {
			VERSION = 'latest'
			PROJECT = 'udacitycapstone-nextcloud'
			IMAGE = "$PROJECT"
			ECRURL = "https://627513304485.dkr.ecr.us-west-2.amazonaws.com/$PROJECT"
			ECRURI = "627513304485.dkr.ecr.us-west-2.amazonaws.com/$PROJECT"
			ECRCRED = 'ecr:us-west-2:nanodegree'
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
