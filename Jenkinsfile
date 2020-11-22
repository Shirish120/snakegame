node ('appserver-ubuntu'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    
    stage ('SAST-scan') {
        build 'SECURITY-SAST-SNYK'
    }
   
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("shirish123/snakegame")
    }
    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            app.push("latest")
        			}
         }
    
    stage('Pull-image-server') {
    
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
    
    stage (DAST-scan) {
        build 'SECURITY-DAST-OWASP_ZAP'
    }
}
