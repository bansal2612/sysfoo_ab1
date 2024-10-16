pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'Building'
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        echo 'testing'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      when {
        branch 'main'
      }
      steps {
        echo 'packaging'
        sh 'mvn package -DskipTests'
      }
    }

    stage('newly-added-stage') {
      parallel {
        stage('newly-added-stage') {
          when {
            branch 'main'
          }
          steps {
            echo 'hey hey'
            sleep 3
          }
        }

        stage('new-stage-2-parallel') {
          when {
            branch 'main'
          }
          steps {
            echo 'see how it goes'
            sleep 2
          }
        }

        stage('stage-new-branch') {
          when {
            branch 'main'
          }
          steps {
            echo 'this is commited to new branch'
          }
        }

        stage('Docker B&p') {
          agent any
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                def commitHash = env.GIT_COMMIT.take(7)
                def dockerImage = docker.build("bansal2612/sysfoo:${commitHash}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

      }
    }

    stage('Deploy to Dev') {
      steps {
        sh 'docker compose up -d'
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}