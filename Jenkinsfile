node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build Docker image') {
  
       app = docker.build("34.125.28.169:5000/radar/radar-driver:${env.BUILD_NUMBER}")
    }

    stage('Test Docker image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }
    stage('Push image to Nexus') {
        sh 'docker login -u admin -p ubnt@117 http://34.125.28.169:5000/repository/radar/'
            app.push("${env.BUILD_NUMBER}")
    }

    stage('Publish  Helm') {
    
        echo "Packing helm chart"
            sh "helm package -d ${WORKSPACE}/radar ${WORKSPACE}/radar/radar-driver/com/et"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
           sh "curl -u admin:ubnt@117 http://34.125.28.169:8081/repository/radar-helm/ --upload-file ${WORKSPACE}/radar/radar-driver/com/et-1.tgz -v"
            
        }
    
}
