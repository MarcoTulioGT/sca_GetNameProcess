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
           sh 'ant -Dapplications.home=$WORKSPACE -Dproject=GetNameProcessCRM  -Dbuild.number=16032017 -DGetNameProcessCRM.compositeName=GetNameProcessCRM  -DGetNameProcessCRM.revision=1.1  -DGetNameProcessCRM.partition=E2E -Ddeployment.plan.environment=cfg_plan'  
                }

        echo 'Compilando...'
          
        echo 'Empaquetando...'
          
        echo 'Desplegando...'
         
      }
    }
    stage('Test') {
      steps {
	         echo 'Testing...'
       sh 'ant -buildfile /CI_QA/ant-jmeter.xml -DworkspacePath=$WORKSPACE -Dnamejmx=Testplan/GetNameProcessCRM -Dambientename=DEV'
       sh 'curl -u admin:#jfrog17Tigo -X PUT "http://172.22.71.6:8081/artifactory/Tigo/tf/om/ic/DEV_Testplan/" -T $WORKSPACE/DEV_Testplan/*.html'
      }
    }
    stage('Artefactory') {
      steps {
        script{
        sh 'curl -u admin:#jfrog17Tigo -X PUT "http://172.22.71.6:8081/artifactory/Tigo/tf/om/ic/sca_GetNameProcessCRM_rev1.0.jar" -T $WORKSPACE/target/sca_GetNameProcessCRM_rev1.0.jar'

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
