pipeline{ 
    agent{ 
        label 'vagrant1'
    }
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
                    sh "docker login harbor.registry.local -u admin -p Harbor12345"
                    sh "docker push ${HARBOR_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                }

            }
        }
        stage("Build the container"){ 
            steps{ 
                script{ 
                    sh "docker container run -d -p 3000:3000 --name ${CONTAINER}"
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