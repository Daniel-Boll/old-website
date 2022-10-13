def build_path = '.'

pipeline {
  agent any
  
  options {
    ansiColor('xterm')
  }
  
  stages {
    stage("Node docker") {
      agent {
        docker {
          image "node:16-alpine"
          args "-p 3000:3000"
        }
      }

      stages {
        stage("Fetch dependencies") {
          steps {
            sh "npm install"
          }
        }

        stage("Build") {
          steps {
            sh "npm run build"
          }
        }
        
        stage("Getting build path") {
          steps {
            sh "pwd > pwd.txt"
            
            script {
              build_path = readFile("pwd.txt").trim()
            }
          }
        }
      }
    }

    stage("Deploy") {
      steps {
        sh "cd ${build_path} && ls"
        sh "chmod +x ./scripts/deploy.sh"
        sh "./scripts/deploy.sh"
      }
    }
  }
}
