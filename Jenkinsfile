pipeline {
  agent any
  stages {
    
    stage('Git') 
    {
	   // properties([pipelineTriggers([[$class: 'GitHubPushTrigger'], pollSCM('H/3 * * * *')])])
      steps 
      {
        git poll: true, url: 'https://github.com/MarcoTulioGT/sca_GetNameProcess.git'
      }
    }
    stage('Build') {
      steps {
        echo 'Verificando...'
        sh 'pwd'
          script {
           def antHome = tool 'Apache Ant(TM) version 1.10.1'
           env.PATH = "${antHome}/bin:${env.PATH}"
           echo 'Compilando aplicación'
           sh 'ant /DevOps/QA.xml applications.home=$WORKSPACE project=sca_GetNameProcessCRM build.number=16032017'
                }

        echo 'Compilando...'
          
        echo 'Empaquetando...'
          
        echo 'Desplegando...'
         
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
