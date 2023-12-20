pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        
        //Set Github Credentials
        
        
        
        // Set your Docker Hub credentials
        DOCKER_HUB_CREDENTIALS = 'docker_hub_mehedi4475'
        
        
        // Set your Docker image details
        DOCKER_IMAGE_NAME = 'mehedi4475/cicd-e2e'
        DOCKER_IMAGE_TAG = 'latest'
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: 'github_username_and_password', 
                url: 'https://github.com/skheya/cicd-end-to-end',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t mehedi4475/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){

           steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")

                    // Log in to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        // Push the Docker image to Docker Hub
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'github_username_and_password', 
                url: 'https://github.com/skheya/cicd-demo-manifests-repo.git',
                branch: 'main'
            }
        }
        

        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([gitUsernamePassword(credentialsId: 'github_username_and_password', gitToolName: 'Default')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/skheya/cicd-demo-manifests-repo.git HEAD:main
                        '''                        
                    }
                }
            }
        }
        
        
        
    }
}