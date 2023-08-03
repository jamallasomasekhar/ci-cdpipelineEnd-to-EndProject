pipeline {
    agent any
    environment {
       IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage ('checkout for git') {
            steps {
                git 'https://github.com/jamallasomasekhar/ci-cdpipelineEnd-to-EndProject.git'
            }
        }
        stage ('build on docker') {
            steps{
                script{
                    sh '''
                    echo "docker bulding"
                    docker build -t jamallasomasekhar/fonicy:${BUILD_NUMBER} .
                    '''
                }
            }
        }
        stage ('docker phush to hub ') {
            steps {
                script {
                    sh '''
                    echo "docker push to repo"
                    docker push jamallasomasekhar/fonicy:${BUILD_NUMBER}
                    ''' 
                }
            }
        }
        stage ('checkout k8s manifest') {
            steps {
                git credentialsId: 'somasekharjamalla', url: 'https://github.com/jamallasomasekhar/ci-cdmanifest.git'
            }
        }
        stage ('edit deploy.yml'){
            steps {
                sh '''
                cat deploy.yml
                sed -i 's/1/${BUILD_NUMBER}/g' deploy.yml
                cat deploy.yml
                git add deploy.yml
                git commit -m 'updated my deploy.yml |jenkins pipeline'
                git remote -v
                git push https://github.com/jamallasomasekhar/ci-cdmanifest.git HEAD:master
                sh'''
            }
        }
    }
}
