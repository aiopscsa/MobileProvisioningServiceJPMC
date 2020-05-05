pipeline {
 agent any
 stages {
    stage('build') {
         steps{
           script {
            echo "Maven Build"
            sleep(time:28,unit:"SECONDS")
           }
         }
    }
    stage('deploy') {
         steps{
           script {
            echo "Maven deploy"
            sleep(time:61,unit:"SECONDS")
           }
         }
    }
  /*
    stage('Blazemeter'){
        steps{
           script {
            loadGeneratorName = env.STAGE_NAME;
            loadGeneratorStartTime = System.currentTimeMillis();
            blazeMeterTest credentialsId:'ae0fe96b-5b1e-4c32-9dc6-06b219da766d',
            serverUrl:'https://blazemeter.ca.com',
            //testId:'6518001',
            testId:'7389604',
            notes:'',
            sessionProperties:'',
            jtlPath:'',
            junitPath:'',
            getJtl:false,
            getJunit:false
            loadGeneratorEndTime = System.currentTimeMillis();

                         map = [jenkinsPluginName: "CAAPM"];
          }
        }
    }
    */
    
  stage ('Test - Selenium') {
   steps {
    script {
       echo "Running the Selenium Test Script"
       sh "./jenkinsSeleniumRunner.sh"
    }
   }
  }
  
 /*    
    stage('CAAPMPerformanceComparator') {
        steps { 
             caapmplugin performanceComparatorProperties: "${env.WORKSPACE}/properties/performance-comparator.properties",
             loadGeneratorStartTime: "$loadGeneratorStartTime",
             loadGeneratorEndTime: "$loadGeneratorEndTime",
             loadGeneratorName: "$loadGeneratorName",

                            attribsStr: "$map";
        }
    }  
    */
  /*
     stage ('Publish CA APM Comparison Reports') {
        steps{
      echo " chart folder is ${env.BUILD_NUMBER}/"
      echo " ....  ${env.JOB_NAME}/"

     publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: false, reportDir: "${env.BUILD_NUMBER}/", reportFiles: 'chart-output.html', reportName: 'CA APM Comparison Reports', reportTitles: ''])
}

   } 
 */
   
 }
}
