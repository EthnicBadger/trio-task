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
                echo "Hello, Jenkins is working"
                docker build -t 
                '''
            }
        }
stage('Push Images') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is working"
                chmod +x run.sh
                ./run.sh 
                hostname
                '''
            }
        }
        stage('Deploy Containers') {
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
