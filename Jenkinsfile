def notifyStarted() {
  // send to email
  emailext (
      subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """
STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':

 
        
Check console output at "${env.JOB_NAME} [${env.BUILD_NUMBER}] "

 """,
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}


def notifyStarted() { /* .. */ }

def notifySuccessful() {
  slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

  hipchatSend (color: 'GREEN', notify: true,
      message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
    )

  emailext (
      subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """
SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':

 
        
Check console output at "${env.JOB_NAME} [${env.BUILD_NUMBER}] "

 """,
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}

node {
    notifyStarted()
    stage('Clone sources') {
        git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
    }
    stage('Read config') {
       

        def config_yaml = readYaml file: './config.yml'
        
        print config_yaml.notifications
    }
      notifySuccessful()
}
