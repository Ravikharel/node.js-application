pipeline{ 
    agent any
    environment{ 
        HARBOR_URL = 'harbor.registry.local/nodejs-app'
        IMAGE_NAME = 'new'
        IMAGE_TAG = "v1"
        CONTAINER = "node-container"
    }
    stages{ 
        stage('Build Image'){ 
            steps{ 
                script{ 
                    sh 'ip a'
                    sh 'docker build -t ${HARBOR_URL}/${IMAGE_NAME}:${IMAGE_TAG} .'
                }
                
            }
        }
        stage('Push the image to the Harbor'){ 
            steps{ 
                script{ 
                    sh "echo Harbor12345 | docker login harbor.registry.local -u admin --password-stdin"
                    sh "docker push ${HARBOR_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                }

            }
        }
        stage("Build the container"){ 
            agent { 
                label 'vagrant1'
            }
            steps{ 
                script{ 
                    sh "echo Harbor12345 | docker login harbor.registry.local -u admin --password-stdin"
                    sh "docker pull ${HARBOR_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker container run -d -p 3000:3000 --name ${CONTAINER}  ${HARBOR_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
    post { 
        success{ 
            echo " Sucess!!"
        }
        failure{ 
            echo "failure!"
        }
    }
}