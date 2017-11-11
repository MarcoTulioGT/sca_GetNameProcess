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
           sh 'ant -Dapplications.home=$WORKSPACE -Dproject=GetNameProcessCRM  -Dbuild.number=16032017 -DGetNameProcessCRM.compositeName=GetNameProcessCRM  -DGetNameProcessCRM.revision=1.0  -DGetNameProcessCRM.partition=E2E -Ddeployment.plan.environment=cfg_plan'  
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
        sh 'curl -u myUser:myP455w0rd! -X PUT "http://172.22.71.6:8081/artifactory/tf/om/ic/sca_GetNameProcessCRM_rev1.0.jar" -T $WORKSPACE/target/sca_GetNameProcessCRM_rev1.0.jar'

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
