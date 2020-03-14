node {
    
    stage('Clone sources') {
        git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
    }
    stage('Read config') {
       

        config_yaml = readYaml file: './config.yml'
        mailRecipients = "log@1ng.me"
        // print config_yaml.notifications
        notificationsSection=config_yaml.notifications
        truemailRecipient= config_yaml.notifications.email.recipients
        jobName = currentBuild.fullDisplayName
        // notifyCheck()
        // notifyEmail()

    }
    stage ('Build'){}
        print config_yaml.build
    stage ('Database'){}
        print config_yaml.database
    stage ('Deploy'){}
        print config_yaml.deploy
    stage ('Test'){}
        print test
      
}






def notifyEmail() {
        emailext body: '''${SCRIPT, template="groovy-html.template"}''',
        mimeType: 'text/html',
        subject: "[Jenkins] ${jobName} ${truemailRecipient}",
        to: "${mailRecipients}",
        replyTo: "${mailRecipients}",



        recipientProviders: [[$class: 'CulpritsRecipientProvider']]
}

