#!groovy

def loadValuesYaml(){
  def valuesYaml = readYaml (file: './config.yml')
  return valuesYaml;
}

def notifyEmail(truemailRecipient) {
        emailext body: '''${SCRIPT, template="groovy-html.template"}''',
        mimeType: 'text/html',
        subject: "[Jenkins] ${jobName} ${truemailRecipient}",
        to: "${mailRecipients}",
        replyTo: "${mailRecipients}",



        recipientProviders: [[$class: 'CulpritsRecipientProvider']]
}


def transformIntoStep(inputString) {
    // We need to wrap what we return in a Groovy closure, or else it's invoked
    // when this method is called, not when we pass it to parallel.
    // To do this, you need to wrap the code below in { }, and either return
    // that explicitly, or use { -> } syntax.
    // def 
    return {
        node {
            echo inputString
        }
    }
}

def makeTest(mapa){
    def folder = "${env.STAGE_NAME}Folder"
    def command = "${env.STAGE_NAME}Command"
    def shcommand = mapa."${command}"
    // println shcommand
    // println folder
    // def folder = ""+test+"Folder"
    // println folder
    // println mapa.get('testFolder')
    script{
        dir (mapa."${folder}"){
            sh "${shcommand}"
        }
    }
    // println mapa."${folder}"
    

    // mapa.each{
    //     if it.getKey()="testFolder"
    //     println it
    // }

}

pipeline {
    agent any
    stages {
        stage('preparing') {
            steps {
                git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
                 script{
                    valuesYaml = loadValuesYaml()
                    truemailRecipient = valuesYaml.notifications.email.recipients
                    }
            }
        }

        stage('build') {
            steps {
                script{
                    dir (valuesYaml.build.projectFolder){
                        sh "${valuesYaml.build.buildCommand}"
                    }
                }
            }
        }
        stage('database') {
            steps {
                script{
                    dir (valuesYaml.database.databaseFolder){
                        sh "${valuesYaml.database.databaseCommand}"
                    }
                }
            }
        }
        stage('test') {
            steps {
                script{
                         def branches = [:]
                        i = 0
                        valuesYaml.test.each{

                        branches["\u001B[31mbranch${i}\u001B[0m"] = {
                            dir (it.testFolder){
                                sh "${it.testCommand}"
                            }

                        }
                        i++
                        }     
                        parallel branches                                          
                    }
                }                
            }
    }

    post {
        
        always {
            echo 'This will always run'
        }
        success {
            emailext (
                to: "log@1ng.me",
                mimeType: 'text/html',
                subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
        failure {
                emailext (
                    to: "log@1ng.me",
                    mimeType: 'text/html',
                    subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p></br>This email for ${truemailRecipient}""",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider']]
                )
            
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }


}