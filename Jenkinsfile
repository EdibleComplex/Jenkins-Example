pipeline {
    agent any
    
    stages {
      stage("checkout") {
        steps {
          sh "ls"
          git branch:'main', url:'https://github.com/g0t4/course3-jenkins-gs-spring-petclinic'
          sh "ls"
        }
      }
      stage("build") {
        steps {
          sh "./mvnw package"
        }
      }
      stage("parallel test) {
        parallel testsA: {
          steps {
             sh "echo test set A" 
             sleep 5
             sleep 1
          }
        }, testB: {
            steps {
            sh: "echo test set B"
            sleep 2
          }
        }
      }
      stage("archive") {
          // **/target/*.jar
        steps {
          archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
          jacoco()
          junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
        }
      }
    }
    
    post {
      always {
        emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
            to: 'remote@thelemuria.xyz',
            recipientProviders: [previous()],
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
      }
//      regression { } // used when the build is in a worse state than the prev build
// check the declarative directive generator in the jenkins gui
    }
}
