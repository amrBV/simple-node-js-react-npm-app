pipeline {
    agent {
        docker {
            image 'node:14-alpine'
            args '-p 3000:3000'
        }
    }
environment {
        registry = "amrbv/vuejs"
        registryCredential = 'docker'
        HOME= '.'
        CI = 'true'
}
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
    stage('Artifacts') {
      steps {
        sh 'tar -czf dist.tar.gz ./dist'
        archiveArtifacts artifacts: 'dist.tar.gz', fingerprint: true
      }
    }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
