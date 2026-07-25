pipeline {

    agent any

    options {
        skipDefaultCheckout()
        timestamps()
    }

    environment {
        APP_NAME       = 'production-devops-pipeline'
        IMAGE_NAME     = 'production-devops-pipeline'
        CONTAINER_NAME = 'springboot-container'
        APP_PORT       = '8082'
        SONARQUBE_ENV  = 'SonarQube'
    }

    tools {
        jdk 'JDK17'
        maven 'Maven-3.8.7'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build, Test & JaCoCo') {
            steps {
                dir('app/springboot-app') {
                    sh '''
                        mvn clean verify
                    '''
                }
            }
        }

        stage('Publish Test Results') {
            steps {
                junit 'app/springboot-app/target/surefire-reports/*.xml'
            }
        }

        stage('Archive JaCoCo Report') {
            steps {
                archiveArtifacts artifacts: 'app/springboot-app/target/site/jacoco/**', fingerprint: true
            }
        }

        stage('Publish JaCoCo HTML Report') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'app/springboot-app/target/site/jacoco',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Coverage Report'
                ])
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('app/springboot-app') {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            mvn sonar:sonar \
                            -Dsonar.projectKey=production-devops-pipeline \
                            -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
                        '''
                    }
                }
            }
        }

        stage('Trivy Filesystem Scan') {
            steps {
                sh '''
                    chmod +x scripts/trivy-fs-scan.sh
                    ./scripts/trivy-fs-scan.sh
                '''
            }
        }

        stage('Archive Trivy Filesystem Scan Report') {
            steps {
                archiveArtifacts artifacts: 'reports/*.txt', fingerprint: true
            }
        }

        stage('Package') {
            steps {
                dir('app/springboot-app') {
                    sh '''
                        mvn package -DskipTests
                    '''
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {

                        def app = docker.build(
                            "dnraju7747/production-devops-pipeline:latest",
                            "-f docker/Dockerfile ."
                        )

                    app.push('latest')
                    }
                }
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh '''
                    docker image inspect ${IMAGE_NAME}:${BUILD_NUMBER} >/dev/null
                    echo "Docker image verified successfully."
                    docker images | grep ${IMAGE_NAME}
                '''
            }
        }
        

        stage('Trivy Image Scan') {
            steps {
                withEnv([
                    "IMAGE_NAME=${IMAGE_NAME}",
                    "IMAGE_TAG=latest"
                ]) {
                    sh '''
                        chmod +x scripts/trivy-image-scan.sh
                        ./scripts/trivy-image-scan.sh
                    '''
                }
            }
        }

        stage('Archive Security Reports') {
            steps {
                archiveArtifacts artifacts: 'reports/*.txt', fingerprint: true
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f kubernetes/
                '''
            }
        }


        stage('Verify Rollout') {
            steps {
                sh '''
                    kubectl rollout status deployment/springboot-app --timeout=300s
                '''
            }
        }

        stage('Verify Kubernetes Resources') {
            steps {
                sh '''
                    echo "========== Pods =========="
                    kubectl get pods -o wide

                    echo ""
                    echo "========== Deployments =========="
                    kubectl get deployments

                    echo ""
                    echo "========== Services =========="
                    kubectl get svc
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    kubectl rollout status deployment/springboot-app --timeout=300s

                    kubectl wait \
                        --for=condition=ready pod \
                        -l app=springboot-app \
                        --timeout=300s

                    kubectl exec deployment/springboot-app -- \
                    curl -f http://localhost:8082/actuator/health
                '''
            }
        }
    }

    post {
        success {
            echo '======================================='
            echo 'CI/CD Pipeline Completed Successfully'
            echo 'Application Deployed to Kubernetes'
            echo '======================================='
        }

        failure {
            echo '======================================='
            echo 'Pipeline Failed'
            echo '======================================='
        }

        always {
            cleanWs()
        }
    }
}
