# docker-jenkins-integration-sample

###### Docker [Playlist](https://www.youtube.com/watch?v=Tg2krHXHzBc&list=PLVz2XdJiJQxzMiFDnwxUDxmuZQU3igcBb).
###### Jenkins [Playlist](https://www.youtube.com/watch?v=Nw3UohhcPO0&list=PLVz2XdJiJQxwS0BZUHX34ocLTJtRGSQzN).

pipeline script

pipeline{
    agent any
    tools {
        maven "mymaven"
    }
    stages{
        stage('Build maven'){
        steps{
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/radheyzunjur/DockerImage-Java.git']])
            sh "mvn -Dmaven.test.failure.ignore=true clean package"
             }
            
                              }
        stage('build docker image'){
            steps{
                script{
                    sh 'docker build -t radheyzunjur/docker-jenkins-integration-sample .'
                        }
                 }
                                  }
        stage('docker push'){
                steps{
                    script{
                       sh  'docker login -u "radheyzunjur" -p "Radhey@123" docker.io'

                        sh 'docker push radheyzunjur/docker-jenkins-integration-sample:latest'

                    }
                }
            
            
        }
    }
}
