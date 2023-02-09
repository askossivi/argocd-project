node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build docker image') {
  
       app = docker.build("devtraining/dani-dev")
    }

    stage('Test docker image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push docker image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'Docker_Hub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest-dani-dev', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
