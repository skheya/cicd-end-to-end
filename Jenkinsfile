pipeline {
    
    agent any 
    
    environment {

        
        APP_NAME = "todo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"        
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        
        //Set Github Credentials
        GITHUB_USERNAME = "skheya"
        GITHUB_CREDENTIALS = 'github_username_and_password'
        GITHUB_CI_PATH = "https://github.com/skheya/cicd-end-to-end"
        GITHUB_CD_PATH = "https://github.com/skheya/cicd-demo-manifests-repo.git"

        
        // Set your Docker Hub credentials
        DOCKERHUB_USERNAME = "mehedi4475"
        DOCKER_HUB_CREDENTIALS = 'docker_hub_mehedi4475'
        DOCKER_IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}"
        DOCKER_IMAGE_TAG = 'latest'
    }
    
    stages {

        // stage('Clear Work Space'){
        //     steps{
        //         cleanWs()
        //     }
        // }
        
        // stage('Checkout'){
        //    steps {
        //         git credentialsId: "${GITHUB_CREDENTIALS}", 
        //         url: "${GITHUB_CI_PATH}",
        //         branch: 'main'
        //    }
        // }

        // stage('Build Docker'){
        //     steps{
        //         script{
        //             sh '''
        //             echo 'Buid Docker Image'
        //             docker build -t ${DOCKERHUB_USERNAME}/${APP_NAME}:${BUILD_NUMBER} .
        //             '''
        //         }
        //     }
        // }

        // stage('Push the artifacts'){

        //    steps {
        //         script {
        //             // Build the Docker image
        //             docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")

        //             // Log in to Docker Hub
        //             docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
        //                 // Push the Docker image to Docker Hub
        //                 docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
        //             }
        //         }
        //     }
        // }

        // stage('Delete Docker Build Image'){
        //     steps{
        //         script{
        //             sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
        //             sh "docker rmi ${IMAGE_NAME}:latest"
        //         }
        //     }
        // }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: "${GITHUB_CREDENTIALS}", 
                url: "${GITHUB_CD_PATH}",
                branch: 'main'
            }
        }
        

        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([gitUsernamePassword(credentialsId: "${GITHUB_CREDENTIALS}", gitToolName: 'Default')]) {
                        sh '''

                        git config --global user.email "mehedi4475@gmail.com"
                        git config --global user.name "Mehedi Hasan"

                        cat deploy.yaml
                        sed -i 's/mehedi4475/todo-app:.*/${APP_NAME}:${IMAGE_TAG}/g' deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push ${GITHUB_CD_PATH} HEAD:main
                        '''                        
                    }
                }
            }
        }
        
        
        
    }
}