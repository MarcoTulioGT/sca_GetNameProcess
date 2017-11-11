pipeline {
  agent any
  stages {
    
    stage('Git') 
    {
	   // properties([pipelineTriggers([[$class: 'GitHubPushTrigger'], pollSCM('H/3 * * * *')])])
      steps 
      {
        git poll: true, url: 'https://github.com/MarcoTulioGT/TomcatWS.git'
      }
    }
    stage('Build') {
      steps {
        echo 'Verificando...'
        sh 'pwd'
          script {
           def mvnHome = tool 'Maven_Oracle'
           env.PATH = "${mvnHome}/bin:${env.PATH}"
           echo "var mvnHome='${mvnHome}'"
           echo "var env.PATH='${env.PATH}'"
           echo 'Compilando aplicación'
           sh 'mvn clean'
                }

        echo 'Compilando...'
          sh 'mvn compile'
        echo 'Empaquetando...'
          sh 'mvn install'
        echo 'Desplegando...'
          sh 'mvn tomcat7:deploy'
      }
    }
    stage('Test') {
      steps {
	         echo 'Testing...'
       // sh 'mvn com.smartbear.soapui:soapui-maven-plugin:4.6.1:test'
      }
    }
    stage('Artefactory') {
      steps {
        script{
                   // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
        def server = Artifactory.server "Artifactory_CRM"
        // Create an Artifactory Maven instance.
       def rtMaven = Artifactory.newMavenBuild()
       def buildInfo
           rtMaven.tool = 'Maven_Oracle'
          rtMaven.deployer releaseRepo:'Tigo', server: server
           buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install'
             server.publishBuildInfo buildInfo
        echo 'Deploying....'
        echo "RESULT: ${currentBuild.result}"
        }
      }
	  
   }
	
	
}

    post {
        always {
             echo "RESULT: ${currentBuild.result}"
        }
        failure {
            echo "RESULT: ${currentBuild.result}"
        }
    }
}
