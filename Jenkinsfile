pipeline {
    agent any

    parameters {
        choice(
            choices: 'prod\ndevelop',
            description: 'Select the target environment',
            name: 'ENVIRONMENT'
        )
        choice(
            choices: 'main\ndevelop',
            description: 'Select the target BRANCH',
            name: 'BRANCH'
        )

       choice(
            choices: 'tracrat-webapp\noauth2\ncompany-management\nlocation-management\nattribute-management',
            description: 'Select the Service Image',
            name: 'CONTAINER'
       ) 
       
    }
    environment {
        registry = "ramy1430"
        imageTag  = "0.2.0"
        DOCKERHUB_CREDENTIALS=credentials('tracrat_docker_id')
        gitUrl = ''
    }    
    stages {
        stage('Git Clone') {
            steps {
                script {
                    
                    if (params.CONTAINER in ['oauth2', 'company-management', 'location-management', 'attribute-management']) {
                        gitUrl = 'https://github.com/IkonicIT/gotracrat-cloud-services.git'
                    } else if (params.CONTAINER == 'tracrat-webapp') {
                        gitUrl = 'https://github.com/IkonicIT/gotracrat-web-app.git'
                    }
                    /*withCredentials([
                        usernamePassword(
                            credentialsId: 'git-credentials-dev',
                            usernameVariable: 'GIT_USERNAME',
                            passwordVariable: 'GIT_PASSWORD'
                        )
                    ]) {
                        git branch: params.BRANCH, url: gitUrl, credentialsId: 'git-credentials-dev'
                    }*/
                }
            }
        }
        
        stage('Build  Docker image') {
            when {
                expression { params.ENVIRONMENT == 'develop' }
            }
            steps {
                    script {
                    def imageFullName = "${registry}/${params.CONTAINER}:${imageTag}"
                    echo imageFullName

                    // Set the correct context path for Docker build
                    
                        
                        if (params.CONTAINER == "tracrat-webapp") {
                            //selected Option is tracrat-webapp-https-2.0

                            withCredentials([
                                usernamePassword(
                                credentialsId: 'git-credentials-dev',
                                usernameVariable: 'GIT_USERNAME',
                                passwordVariable: 'GIT_PASSWORD'
                                )
                            ]) {
                                git branch: params.BRANCH, url: gitUrl, credentialsId: 'git-credentials-dev'     
                                def contextPath = "gotracrat-web-app"
                                docker.build(imageFullName, "-f ${contextPath}/Dockerfile .")

                                withCredentials([
                                            usernamePassword(
                                                credentialsId: 'tracrat_docker_id',
                                                usernameVariable: 'DOCKERHUB_USERNAME',
                                                passwordVariable: 'DOCKERHUB_PASSWORD'
                                            )
                                        ]) {
                                            sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                                            sh "docker push ${imageFullName}"
                                        }
                                        
                                        sh "docker stop tracrat-webapp || true"
                                        sh "docker rm tracrat-webapp || true"
                                        sh "sudo docker run --name tracrat-webapp -p 9000:80/tcp ramy1430/tracrat-webapp:latest"
                            }

                        }
                        
                        if (params.CONTAINER == "oauth2") {
                        

                            withCredentials([
                                usernamePassword(
                                credentialsId: 'git-credentials-dev',
                                usernameVariable: 'GIT_USERNAME',
                                passwordVariable: 'GIT_PASSWORD'
                                )
                            ]) {
                                //def dockerfilePath = 'oauth2/Dockerfile'
                                git branch: params.BRANCH, url: gitUrl, credentialsId: 'git-credentials-dev'
                                dir('oauth2') {
                                        //Maven compile
                                        sh 'mvn clean install -DskipTests=true'
                                        
                                        docker.build(imageFullName, "-f Dockerfile .")
                                        // Docker push
                                        withCredentials([
                                            usernamePassword(
                                                credentialsId: 'tracrat_docker_id',
                                                usernameVariable: 'DOCKERHUB_USERNAME',
                                                passwordVariable: 'DOCKERHUB_PASSWORD'
                                            )
                                        ]) {
                                            sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                                            sh "docker push ${imageFullName}"
                                        }
                                        
                                        sh "docker stop oauth2 || true"
                                        sh "docker rm oauth2 || true"
                                        sh "docker run --name oauth2 --network=tracrat -p 8081:8081/tcp -d ramy1430/oauth2:0.2.0"
                                        //sh "sleep 30 && docker container logs --since "1m" company-management"
                                }
                            }
                            
                        }
                        if (params.CONTAINER == "company-management") {
                        
                            withCredentials([
                                usernamePassword(
                                credentialsId: 'git-credentials-dev',
                                usernameVariable: 'GIT_USERNAME',
                                passwordVariable: 'GIT_PASSWORD'
                                )
                            ]) {
                                //def dockerfilePath = 'oauth2/Dockerfile'
                                git branch: params.BRANCH, url: gitUrl, credentialsId: 'git-credentials-dev'
                                dir('company-management') {
                                        //Maven compile
                                        sh 'mvn clean install -DskipTests=true'
                                        
                                        docker.build(imageFullName, "-f Dockerfile .")
                                        // Docker push
                                        withCredentials([
                                            usernamePassword(
                                                credentialsId: 'tracrat_docker_id',
                                                usernameVariable: 'DOCKERHUB_USERNAME',
                                                passwordVariable: 'DOCKERHUB_PASSWORD'
                                            )
                                        ]) {
                                            sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                                            sh "docker push ${imageFullName}"
                                        }
                                        
                                        sh "docker stop company-management || true"
                                        sh "docker rm company-management || true"
                                        sh "docker run --name company-management --network=tracrat -p 8085:8085/tcp -d ramy1430/company-management:0.2.0"
                                        //sh "sleep 30 && docker container logs --since "1m" company-management"
                                }
                            }

                            
                        }
                        if (params.CONTAINER == "location-management") {
                        
                            withCredentials([
                                usernamePassword(
                                credentialsId: 'git-credentials-dev',
                                usernameVariable: 'GIT_USERNAME',
                                passwordVariable: 'GIT_PASSWORD'
                                )
                            ]) {
                                //def dockerfilePath = 'oauth2/Dockerfile'
                                git branch: params.BRANCH, url: gitUrl, credentialsId: 'git-credentials-dev'
                                dir('location-management') {
                                        //Maven compile
                                        sh 'mvn clean install -DskipTests=true'
                                        
                                        docker.build(imageFullName, "-f Dockerfile .")
                                        // Docker push
                                        withCredentials([
                                            usernamePassword(
                                                credentialsId: 'tracrat_docker_id',
                                                usernameVariable: 'DOCKERHUB_USERNAME',
                                                passwordVariable: 'DOCKERHUB_PASSWORD'
                                            )
                                        ]) {
                                            sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                                            sh "docker push ${imageFullName}"
                                        }
                                        
                                        sh "docker stop location-management || true"
                                        sh "docker rm location-management || true"
                                        sh "docker run --name location-management --network=tracrat -p 8087:8087/tcp -d ramy1430/location-management:0.2.0"
                                        //sh "sleep 30 && docker container logs --since "1m" company-management"
                                }
                            }

                            
                        }

                        if (params.CONTAINER == "attribute-management") {
                        
                            withCredentials([
                                usernamePassword(
                                credentialsId: 'git-credentials-dev',
                                usernameVariable: 'GIT_USERNAME',
                                passwordVariable: 'GIT_PASSWORD'
                                )
                            ]) {
                                //def dockerfilePath = 'oauth2/Dockerfile'
                                git branch: params.BRANCH, url: gitUrl, credentialsId: 'git-credentials-dev'
                                dir('attribute-types-management') {
                                        //Maven compile
                                        sh 'mvn clean install -DskipTests=true'
                                        
                                        docker.build(imageFullName, "-f Dockerfile .")
                                        // Docker push
                                        withCredentials([
                                            usernamePassword(
                                                credentialsId: 'tracrat_docker_id',
                                                usernameVariable: 'DOCKERHUB_USERNAME',
                                                passwordVariable: 'DOCKERHUB_PASSWORD'
                                            )
                                        ]) {
                                            sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                                            sh "docker push ${imageFullName}"
                                        }
                                        
                                        sh "docker stop attribute-management || true"
                                        sh "docker rm attribute-management || true"
                                        sh "docker run --name attribute-management --network=tracrat -p 8087:8087/tcp -d ramy1430/attribute-management:0.2.0"
                                        //sh "sleep 30 && docker container logs --since "1m" company-management"
                                }
                            }
                        }

                    
                    } 
                }
                    
        }

    }
}
