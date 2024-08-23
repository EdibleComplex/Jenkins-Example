node {
    stage("checkout") {
    sh "ls"
    git branch:'main', url:'https://github.com/g0t4/course3-jenkins-gs-spring-petclinic'
    sh "ls"
    }
    stage("build") {
        sh "./mvnw package"
    }
    parallel testsA: {
       sh "echo test set A" 
       sleep 5cat /var/jenkins_home/credentials/*.xml | java -jar /var/jenkins_home/jenkins-cli.jar -auth admin:password -s http://localhost:8080/ create-credentials-by-xml system::system::jenkins "(global)" 2>>/var/jenkins_home/setup_errors.txt
       sleep 1
    }, testB: {
        sh: "echo test set B"
        sleep 2
    }
    stage("archive") {
        // **/target/*.jar
        archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
        jacoco()
        junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
    }
    emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
        to: 'remote@thelemuria.xyz',
        recipientProviders: [previous()],
        subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
    
}
