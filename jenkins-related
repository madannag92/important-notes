println(hudson.util.Secret.fromString("{XXX}").getPlainText()) ->>XXX is where u need to place the credential from jenkins and need to run this in script console in jenkins

 stage('SCM to build') {
        git credentialsId: 'bitbucket-access',
                url: 'ssh://git@ritscm.com/ritiac/ecr-automation.git',
                branch: 'master'
---------------------------------------
Sonarqube stage scan
 stage("SonarQube analysis") { 
            def scannerHome = tool 'SonarQubeScanner';
            withSonarQubeEnv(credentialsId: 'sonarqube-auth-token') { 
                sh """
                  $scannerHome/bin/sonar-scanner \
                    -Dsonar.projectKey=tppm-web-frontend \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://sonar.com \
                """  
            }
        }
---------------------------
sample docker login pipeline
pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t shanem/spring-petclinic:latest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push shanem/spring-petclinic:latest'
        }
      }
    }
  }
}
