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
      sh 'npm run build'
      sh 'ls -al'
    }
  }
  stage('Test') {
    steps {
        echo 'Testing'
    }
  }
  stage('Deploy') {
    steps {
        sshagent (credentials: ['100-monosparta-loadbalancer']) {
          sh "scp -r build deploy@10.2.9.110:~/"
          sh """
          ssh -o StrictHostKeyChecking=no -T deploy@10.2.9.110  << EOF
          echo deploy | sudo -S mv ~/build/**.* /var/www/gohiking-web
          exit
          """
        }
      }
    }
  }    
  post {
      always {
          archiveArtifacts artifacts: 'frontend.zip', fingerprint: true
      }
  }
}
