node {
    notifyStarted()
    stage('Clone sources') {
        git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
    }
    stage('Read config') {
       

        def config_yaml = readYaml file: './config.yml'
        def mailRecipients = "log@1ng.me"
        print config_yaml.notifications

        def jobName = currentBuild.fullDisplayName

        notifySuccessful()

    }
      
}


def notifyEmail() {
        emailext body: '''${SCRIPT, template="groovy-html.template"}''',
        mimeType: 'text/html',
        subject: "[Jenkins] ${jobName}",
        to: "${mailRecipients}",
        replyTo: "${mailRecipients}",
        recipientProviders: [[$class: 'CulpritsRecipientProvider']]
}
def notifyStarted() {
  // send to email
  emailext (
      to: "${env.mailRecipients}",
      subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """
STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':

 
        
Check console output at "${env.JOB_NAME} [${env.BUILD_NUMBER}] "

 """,
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}



def notifySuccessful() {

  emailext (
      subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """
SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':

 
        
Check console output at "${env.JOB_NAME} [${env.BUILD_NUMBER}] "

 """,
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}


