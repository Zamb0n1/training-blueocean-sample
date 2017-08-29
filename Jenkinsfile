pipeline {
  agent {
    docker {
      image 'bitwiseman/training-blueocean-sample'
      args '-u root -v $HOME/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Backend": {
            sh './jenkins/test-backend.sh'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Frontend": {
            sh './jenkins/test-frontend.sh'
            
          },
          "Performance": {
            sh './jenkins/test-performance.sh'
            
          },
          "Static": {
            sh './jenkins/test-static.sh'
            
          }
        )
      }
    }
    stage('Deploy to Dev') {
      steps {
        sh './jenkins/deploy.sh dev'
      }
    }
    stage('Deploy to Stage') {
      steps {
        input(message: 'Deploy to staging?', ok: 'Fire Away!')
        sh './jenkins/deploy.sh staging'
        sh 'echo Notifying appropriate team members!'
      }
    }
  }
}