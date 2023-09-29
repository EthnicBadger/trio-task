pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is working"
                '''
            }
        }
      stage('Build Images') {
            steps {
                sh ''' 
                echo "Hello, Starting the image build process"

                # BUILDING EACH IMAGE IN TURN
                # LATEST PLUS A VERSION NUMBERED IMAGE

                docker build -t ethnicbadger/trio-task-db:latest ./db
                docker build -t ethnicbadger/trio-task-db:${BUILD_NUMBER} ./db 

                docker build -t ethnicbadger/trio-task-app:latest ./flask-app
                docker build -t ethnicbadger/trio-task-app:${BUILD_NUMBER} ./flask-app

                docker build -t ethnicbadger/trio-task-proxy:latest ./nginx
                docker build -t ethnicbadger/trio-task-proxy:${BUILD_NUMBER} ./nginx
                '''
            }
        }
stage('Push Images') {
            steps {
                sh ''' 
                echo "Hello, Starting to push the images to DockerHub"

                # PUSH THE NEW IMAGES TO DOCKER HUB

                docker push ethnicbadger/trio-task-db
                docker push ethnicbadger/trio-task-db:${BUILD_NUMBER}

                docker push ethnicbadger/trio-task-app
                docker push ethnicbadger/trio-task-app:${BUILD_NUMBER}

                docker push ethnicbadger/trio-task-proxy
                docker push ethnicbadger/trio-task-proxy:${BUILD_NUMBER}

                '''
            }
        }

stage('clean up Jenkins') {
            steps {
                sh ''' 
                echo "Hello, Starting clean up of Jenkins"

                docker rmi ethnicbadger/trio-task-db
                docker rmi ethnicbadger/trio-task-db:${BUILD_NUMBER}

                docker rmi ethnicbadger/trio-task-app
                docker rmi ethnicbadger/trio-task-app:${BUILD_NUMBER}

                docker rmi ethnicbadger/trio-task-proxy
                docker rmi ethnicbadger/trio-task-proxy:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy Containers') {
            environment {
                MYSQL_ROOT_PASSWORD = credentials('MYSQL_ROOT_PASSWORD')
            }
            steps {
                sh ''' 
                echo "Hello, Jenkins is still working"

                # NOW WE'RE GOING TO DEPLOY THE CONTAINERS BASED ON THE IMAGES GENERATED

                # INVOKE SSH CONNECTION TO APP SERVER VM
                ssh adam-jenkins-vm2 << EOF
                
                docker pull ethnicbadger/trio-task-db
                docker pull ethnicbadger/trio-task-app
                docker pull ethnicbadger/trio-task-proxy

                # CHECK IF NETWORK EXISTS, IF YES - SLEEP, IF NOT - CREATE IT
                docker network inspect trio && sleep 1 || network create trio

                # CHECK IF VOLUME EXISTS, IF YES - SLEEP, IF NOT - CREATE IT
                docker volume inspect trio && sleep 1 || docker volume create trio

                #  TRY TO STOP SQL, IF STOP THEN REMOVE, IF WE DON'T STOP TRY REMOVE ANYWAY,  IF NOT SUCCESSFUL THEN SLEEP ANYWAY
                docker stop mysql && (docker rm mysql) || (docker rm mysql && sleep 1 || sleep 1)
                docker stop flask-app && (docker rm flask-app) || (docker rm flask-app && sleep 1 || sleep 1)
                docker stop proxy && (docker rm nginx) || (docker rm nginx && sleep 1 || sleep 1)


                docker run -d -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} -v trio:/var/lib/mysql --network trio --name mysql ethnicbadger/trio-task-db
                docker run -d -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} --network trio --name flask-app ethnicbadger/trio-task-app
                docker run -d -p 80:80 --network trio --name nginx ethnicbadger/trio-task-proxy
                '''
            }
        }
        
    }
}
