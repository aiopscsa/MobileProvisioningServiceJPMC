pipeline {
 agent any
 stages {
      stage('Validate-Feature Flag') {
         steps{
           script {
            echo "Validating Feature Flag"
            SLEEP_TIME = Math.abs( new Random().nextInt() % (13 - 9) ) + 9;
            sh "sleep $SLEEP_TIME"
           }
        }
    }
    stage('Deploy-Feature Flag') {
         steps{
           script {
            echo "Enabling Feature Flag"
            SLEEP_TIME = Math.abs( new Random().nextInt() % (18 - 13) ) + 13;
            sh "sleep $SLEEP_TIME"
           }
        }
    }
    
    stage('Blazemeter') {
         steps{
           script {
            dir ("/root/selenium") {
            echo "running Tests"
             
             try {
              echo "ensure any prev running slow UC is shut. Ignore any error due to this"
              //sh "kubectl delete -f selenium-standalone-slow.yml -n selenium"
             } catch (err) {
                //ignore
             }
             //sh "kubectl create -f selenium-standalone-slow.yml -n selenium"
             sleep(time:10,unit:"SECONDS")
             
            loadGeneratorName = env.STAGE_NAME;
            loadGeneratorStartTime = System.currentTimeMillis();
            blazeMeterTest credentialsId:'aa2b41eb-23f3-4045-afe5-374a0b28d202',
            serverUrl:'https://blazemeter.ca.com',
            //testId:'6518001',
            testId: '8026832',
            notes:'',
            sessionProperties:'',
            jtlPath:'',
            junitPath:'',
            getJtl:false,
            getJunit:false
            loadGeneratorEndTime = System.currentTimeMillis();

                         map = [jenkinsPluginName: "CAAPM"];
             
           sleep(time:10,unit:"SECONDS") 
             
             
             echo "Done Blazemeter Test"
         } 
        }
      }
    }
  
     
    stage('CA APM Plugin') {
        steps { 
             caapmplugin performanceComparatorProperties: "${env.WORKSPACE}/caapm-performance-comparator/properties/performance-comparator.properties",
              loadGeneratorStartTime: "${loadGeneratorStartTime}",
             loadGeneratorEndTime: "$loadGeneratorEndTime",
             loadGeneratorName: "$loadGeneratorName",
                            attribsStr: "$map";
        }
    }  
    
  
     stage ('Publish CA APM Comparison Reports') {
        steps{
      echo " chart folder is ${env.BUILD_NUMBER}/"
      echo " ....  ${env.JOB_NAME}/"
         
         sleep(time:5,unit:"SECONDS")

     publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: false, reportDir: "${env.BUILD_NUMBER}/", reportFiles: 'chart-output.html', reportName: 'CA APM Comparison Reports', reportTitles: ''])
    }

      
   } 
 
  }
 }
