pipeline {
       agent any
       git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
       

        config_yaml = readYaml file: './config.yml'
        mailRecipients = "log@1ng.me"
        notificationsSection=config_yaml.notifications
        truemailRecipient= config_yaml.notifications.email.recipients
        jobName = currentBuild.fullDisplayName
        // notifyCheck()
        // notifyEmail()

    stage ('Build'){
        dir (config_yaml.build.projectFolder) {
            sh "${config_yaml.build.buildCommand}"
        }
    }
    stage ('Database'){
        dir (config_yaml.database.databaseFolder) {
            sh "${config_yaml.database.databaseCommand}"
        }
    }
    stage ('Deploy'){
        dir (config_yaml.build.projectFolder) {
            sh "${config_yaml.deploy.deployCommand}"
        }
    }
    stage ('Test'){
        def branches = [:]
        config_yaml.test.each {
            dir (it.testFolder){
            print it.name
            sh "${it.testCommand}"
            }
        parallel branches
    }
    }
      
}






def notifyEmail() {
        emailext body: '''${SCRIPT, template="groovy-html.template"}''',
        mimeType: 'text/html',
        subject: "[Jenkins] ${jobName} ${truemailRecipient}",
        to: "${mailRecipients}",
        replyTo: "${mailRecipients}",



        recipientProviders: [[$class: 'CulpritsRecipientProvider']]
}

