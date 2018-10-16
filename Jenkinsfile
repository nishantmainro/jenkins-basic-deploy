node {
 try {
  
  stage('Preparation') {
    git 'https://github.com/vgadikota/apigee_CI_config_proxy_sharedflow.git'
   mvnHome = tool 'LOCALMAVEN'
  }

try {
  stage('Deploy to Test') {
   // Run the maven build
   sh "'${mvnHome}/bin/mvn' -f $WORKSPACE/proxies/pom.xml install -Ptest -Dorg=nishantmainro-eval -Dusername=nishant.mainro@fiserv.com -Dpassword=Password@123 -Dapigee.config.options=update -X validate"
   cucumber fileIncludePattern: '**/proxies/payment-v2/target/reports.json', sortingMethod: 'ALPHABETICAL'
   }
}catch (e) {
   //if tests fail, we can use a shell script which has 3 APIs to undeploy, delete current revision & deploy previous revision
   //sh "$WORKSPACE/undeploy.sh"
   throw e
  } finally {
   
  }
  
try {
  stage('Deploy to Production') {
   // Run the maven build
   sh "'${mvnHome}/bin/mvn' -f $WORKSPACE/proxies/pom.xml install -Pprod -Dorg=nishantmainro-eval -Dusername=nishant.mainro@fiserv.com -Dpassword=Password@123 -Dapigee.config.options=update -X validate"
  }
  } catch (e) {
   //if tests fail, we can use a shell script which has 3 APIs to undeploy, delete current revision & deploy previous revision
   //sh "$WORKSPACE/undeploy.sh"
   throw e
  } finally {
   
  }
 } catch (e) {
  currentBuild.result = 'FAILURE'
  throw e
 } finally {
  
 }
}

