pipeline {
 agent any
 stages {
    stage('build') {
         steps{
           script {
            echo "Maven Build"
            sleep(time:15,unit:"SECONDS")
           }
         }
    }
    stage('deploy') {
         steps{
           script {
            echo "Maven deploy"
            sleep(time:25,unit:"SECONDS")
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
    
    stage('Selenium') {
         steps{
           script {
            dir ("/root/selenium") {
            echo "running Selenium Test"
           sh "kubectl create -f selenium-standalone-slow.yml -n selenium"
             sleep(time:10,unit:"SECONDS")
             
            loadGeneratorName = env.STAGE_NAME;
            loadGeneratorStartTime = System.currentTimeMillis();
            blazeMeterTest credentialsId:'aa2b41eb-23f3-4045-afe5-374a0b28d202',
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
             
           sh "kubectl delete -f selenium-standalone-slow.yml -n selenium"
             echo "Done Blazemeter Test"
         } 
        }
      }
    }
  
     
    stage('CAAPMPerformanceComparator') {
        steps { 
             caapmplugin performanceComparatorProperties: "${env.WORKSPACE}/caapm-performance-comparator/properties/performance-comparator.properties",
             loadGeneratorStartTime: "$loadGeneratorStartTime",
             loadGeneratorEndTime: "$loadGeneratorEndTime",
             loadGeneratorName: "$loadGeneratorName",

                            attribsStr: "$map";
        }
    }  
    
  
     stage ('Publish CA APM Comparison Reports') {
        steps{
      echo " chart folder is ${env.BUILD_NUMBER}/"
      echo " ....  ${env.JOB_NAME}/"

     publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: false, reportDir: "${env.BUILD_NUMBER}/", reportFiles: 'chart-output.html', reportName: 'CA APM Comparison Reports', reportTitles: ''])
}

   } 
 
   
 }
 }
