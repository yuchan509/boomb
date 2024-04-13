pipeline {
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'kaniko-pod.yaml'
        }
    }
    stages {
        stage('Prepare') {
            steps {
                sh 'echo Preparing the environment'
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Images') {
            parallel {
                stage('Module API') {
                    environment {
                        IMAGE_NAME = 'test-api'
                        IMAGE_TAG = "0.0.${BUILD_NUMBER}"
                    }
                    steps {
                        container('kaniko') {
                            script {
                                try {
                                    sh """
                                    /kaniko/executor \
                                        --context=dir://${WORKSPACE}/module-api \
                                        --dockerfile=${WORKSPACE}/module-api/Dockerfile \
                                        --destination=${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} \
                                        --destination=${ECR_REGISTRY}/${IMAGE_NAME}:latest
                                    """
                                    echo "Image ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} and latest tag successfully built and pushed."
                                } catch (Exception e) {
                                    echo "Error in building or pushing with Kaniko: ${e.message}"
                                    throw e
                                }
                            }
                        }
                    }
                }
                stage('Module Batch') {
                    environment {
                        IMAGE_NAME = 'test-batch'
                        IMAGE_TAG = "0.0.${BUILD_NUMBER}"
                    }
                    steps {
                        container('kaniko') {
                            script {
                                try {
                                    sh """
                                    /kaniko/executor \
                                        --context=dir://${WORKSPACE}/module-batch \
                                        --dockerfile=${WORKSPACE}/module-batch/Dockerfile \
                                        --destination=${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} \
                                        --destination=${ECR_REGISTRY}/${IMAGE_NAME}:latest
                                    """
                                    echo "Image ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} and latest tag successfully built and pushed."
                                } catch (Exception e) {
                                    echo "Error in building or pushing with Kaniko: ${e.message}"
                                    throw e
                                }
                            }
                        }
                    }
                }
                // 추가 모듈 구성에 따른 병렬 스테이지 추가 가능
            }
        }
    }
}
