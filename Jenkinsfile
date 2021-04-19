pipeline {
  agent {
    dockerfile {
      filename 'Dockerfile'
    } 
  }
  environment {
      npm_config_cache = 'npm-cache'
  }
  stages { 
    stage("Version") {
      steps {
        sh 'node --version'
        sh 'npm --version'
      }
    }
    stage("Build") {
      steps {
        sh 'npm install'
        sh 'CI="" npm run build'
        sh 'ls -al'
        zip zipFile: 'fontend.zip', archive: true, dir: ''
      }
    }
    stage('Test') {
      steps {
          echo 'Testing'    
      }
    }
    stage('Build Job') {
      steps {
        build job: 'go-hiking-web-delpoy-build', parameters: [
            string(name: 'go-hiking-web-delpoy', value: env.NAME)
        ], wait: false
      }
    }
  }    
  post {
      always {
          archiveArtifacts artifacts: 'frontend.zip', fingerprint: true
      }
  }
}
