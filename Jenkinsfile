pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is working"
                echo "webhook"
                echo "webhook2"
                echo "build6?"
                echo "build7?"
                '''
            }
        }
      stage('Pull_Repo') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is working"
                chmod +x run.sh
                ./run.sh 
                hostname
                '''
            }
        }
stage('BI trio-app') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is working"
                chmod +x run.sh
                ./run.sh 
                hostname
                '''
            }
        }
        stage('BI trio-db') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is still working"
                chmod +x run2.sh
                ./run2.sh 
                hostname
                '''
            }
        }
        stage('BI trio-nginx') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is still working"
                chmod +x run2.sh
                ./run2.sh 
                hostname
                '''
            }
        }
        stage('SetNet') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is still working"
                chmod +x run2.sh
                ./run2.sh 
                hostname
                '''
            }
        }
    }
}
