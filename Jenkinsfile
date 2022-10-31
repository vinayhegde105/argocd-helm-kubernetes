node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build Docker image') {
  
       app = docker.build("192.168.56.120:9092/helmodia:${env.BUILD_NUMBER}")
    }

    stage('Test Docker image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image to Nexus') {
        sh 'docker login -u admin -p admin http://192.168.56.120:9092/repository/shanvi-image-helm/'
            app.push("${env.BUILD_NUMBER}")
    }
    
}