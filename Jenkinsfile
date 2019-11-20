pipeline {
        agent any
        
        deleteDir()
        
        stage("Checkout") {
            checkout scm
        }
        stage ("Build Container"){
         
        }
        stage ("Next Stage"){
         
        }   
        stage ("Next Stage"){
         
        }   

        
        stage ("Automated Test Cases"){
          
          // give the container 10 seconds to initialize the web server
          sh "sleep 10"

          // connect to the webapp and verify it listens and is connected to the db
          //
          // to get IP of jenkins host (which must be the same container host where dev instance runs)
          // we passed it as an environment variable when starting Jenkins.  Very fragile but there is
          // no other easy way without introducing service discovery of some sort
          echo "Check if webapp port is listening and connected with db"
          // sh "curl http://192.168.42.5:30001/v1/ping -o curl.out"
          // sh "cat curl.out"
          // sh "awk \'/true/{f=1} END{exit!f}\' curl.out"
          echo "<<<<<<<<<< Access this test build at http://192.168.42.5:30001 >>>>>>>>>>"        
        }
        def push = ""
        stage ("Manual Test & Approve Push to Production"){
          // Test instance is online.  Ask for approval to push to production.
          // notifyBuild('APPROVAL-REQUIRED')
          push = input(
            id: 'push', message: 'Push to production?', parameters: [
              [$class: 'ChoiceParameterDefinition', choices: 'Yes\nNo', description: '', name: 'Select yes or no']
            ]
          )
        }
        
        stage('Deploy in Production') {

        }
        stage('Delete test instance') {

        }
        
        stage('finale notification'){
            notifyBuild(currentBuild.result)
        }
}


def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
