pipeline {
    agent none
    
    stages {
        stage('CLONE GIT REPOSITORY') {
            agent {
                label 'ubuntu-us-appserver-2140-60'
            }
            steps {
                checkout scm
            }
        }  

        stage('SCA-SAST-SNYK-TEST') {
            agent any
            steps {
                script {
                    snykSecurity(
                        snykInstallation: 'Snyk',
                        snykTokenId: 'Synkid',
                        severity: 'critical'
                    )
                }
            }
        }

        stage('SonarQube Analysis') {
            agent {
                label 'ubuntu-us-appserver-2140-60'
            }
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=gameapp \
                            -Dsonar.sources=."
                    }
                }
            }
        }

        stage('BUILD-AND-TAG') {
            agent {
                label 'ubuntu-us-appserver-2140-60'
            }
            steps {
                script {
                    def app = docker.build("amalan06/sgameimg")
                    app.tag("latest")
                }
            }
        }

        stage('POST-TO-DOCKERHUB') {    
            agent {
                label 'ubuntu-us-appserver-2140-60'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials') {
                        def app = docker.image("amalan06/sgameimg")
                        app.push("latest")
                    }
                }
            }
        }

        stage('DEPLOYMENT') {    
            agent {
                label 'ubuntu-us-appserver-2140-60'
            }
            steps {
                sh "docker-compose down"
                sh "docker-compose up -d"   
            }
        }
    }
}




// node('ubuntu-us-appserver-2140-60')
// {
//     def app
//     stage('Cloning Git')
//     {
//         checkout scm
//     }

//     stage('SNYK SCA SAST TEST')
//     {
//         snykSecurity(
//             snykInstallation: 'Snyk',
//             snykTokenId: 'snyk_id',
//             severity: 'critical'
//         )
//     }

//     stage('SonarQube Analysis')
//     {
//        agent
//         {
//             label 'ubuntu-us-appserver-2140-60'
//         }
//         steps
//         {
//             script
//             {
//                def scannerHome = tool 'SonarQubeScanner'
//                withSonarQubeEnv('sonarqube')
//                 {
//                     sh "${scannerHome}/bin/sonar-scanner \
//                        -Dsonar.projectKey=gameapp \
//                        --Dsonar.sources=."
//                 }
//             }
//         }
//     }
    

//     stage('Build-and-Tag')
//     {
//         app = docker.build('amalan06/sgameimg')
//     }

//     stage('Post-to-dockerhub')
//     {
//         docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
//         {
//             app.push('latest')
//         }
//     }

//     stage('Deploy')
//     {
//         sh "docker-compose down"
//         sh "docker-compose up -d"
//     }


// }
