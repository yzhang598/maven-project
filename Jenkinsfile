pipeline {
  agent {
    label 'jenkins_node1'
  }


    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yzhang598/maven-project.git']]])

            }
        }
          
        stage('Build') {
            steps {
            
                // Run Maven on a Unix agent.
                sh "mvn clean package"

            }
        }
              
        stage('Build Docker Image') {
            steps {
            
                // create docker build
                sh "docker build -t webapp:v${BUILD_NUMBER} ."

            }
        }
          
      stage('Push image to Docker Hub') {
          steps {
              sh "docker tag webapp:v${BUILD_NUMBER} yzhang598/webapp:v${BUILD_NUMBER}"
              sh "docker push yzhang598/webapp:v${BUILD_NUMBER}"
            }
        }
        
      stage('Runthe webapp container') {
          steps {
              sh "docker run --name webapp_v${BUILD_NUMBER} -p 8087:8080 -d yzhang598/webapp:v${BUILD_NUMBER}"
            }
        }
    }
}
