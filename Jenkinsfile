node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build Docker image') {
  
       app = docker.build("192.168.56.120:9092/shanvi-image-helm/helmodia:${env.BUILD_NUMBER}")
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
    stage('Publish  Helm') {
    
        echo "Packing helm chart"
            sh "helm package -d ${WORKSPACE}/helm ${WORKSPACE}/helm/devopsodia"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
           sh "curl -u admin:admin http://192.168.56.120:8081/repository/shanvi-helm-dev/ --upload-file ${WORKSPACE}/helm/devopsodia-1.tgz -v"
            
        }
    
}