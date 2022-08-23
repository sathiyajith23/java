pipeline {
    agent any
    triggers {
        pollSCM '* * * * *'
    }
    tools {
        maven 'maven 3.8.6'
    }
    stages {
        stage("code checkout") {
            steps {
                echo "========checking out code from github repo========"
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sathiyajith23/java.git']]])
            }
            post {
                success {
                    echo "========Code checkout from Github repo completed========"
                }
                failure {
                    echo "========Code checkout from Github repo failed========"
                }
            }
        }
        stage("Build") {
            steps{
                sh 'mvn -B -DskipTests clean package'
            }
            post{
                success {
                    echo "=========Build completed successfully============="

                }
                failure {
                    echo "==========Build failed=========="
                }
            }

        }
        stage ("execute script") {
            steps{
                echo "Workspace:- $WORKSPACE"
                echo "Job Name :- $JOB_NAME"
                echo "Build ID :- $BUILD_ID"
                echo "Jenkins Home :- $JENKINS_HOME"

            }
        }
        stage("Unit Testing") {
            steps{
                sh 'mvn test'
            }
            post {
                success{
                    echo "======Unit testing completed successfull, publishing report========="
                    junit 'target/surefire-reports/*.xml'
                }
                failure{
                    echo "==========unit test cases failed, report not published===="
                    mail bcc: '', body: 'unit test fail', cc: '', from: '', replyTo: '', subject: 'fail', to: 'sathiyajith23@gmail.com'
                }
            }
        }
        stage("archiveArtifacts") {
            steps {
                archiveArtifacts artifacts: 'target/*jar', followSymlinks: false
            }
        }
        stage ('Check for existence of my-app-1.0.0.jar') {
            steps {
                script {
                    if (fileExists('target/my-app-1.0.0.jar')) {
                        echo "File target/my-app-1.0.0.jar found!"
                    }
                    else {
                        echo "File not found"
                    }
                }
            }
        }
        stage("waiting Time") {
            steps{
                sleep time: 5, unit: 'MILLISECONDS'
            }
        }
    }
}