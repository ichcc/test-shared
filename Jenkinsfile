#!groovy

def loadValuesYaml(){
  def valuesYaml = readYaml (file: './config.yml')
  return valuesYaml;
}

pipeline {
    agent any
   
    stages {
        stage('Preparing') {
            steps {
                 git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
                 script{
                    valuesYaml = loadValuesYaml()
                    // valuesYaml.each{
                    //     println it
                    }
                }
            }
        stage('Build'){
            steps{
                valuesYaml.build.each{
                      dir (it.projectFolder) {
                        sh "${it.buildCommand}"
                        }
                }
            }
        }    
        
        
    }
    
    // post {
    //     always {
    //         echo 'This will always run'
    //     }
    //     success {
    //         echo 'This will run only if successful'
    //     }
    //     failure {
    //         echo 'This will run only if failed'
    //     }
    //     unstable {
    //         echo 'This will run only if the run was marked as unstable'
    //     }
    //     changed {
    //         echo 'This will run only if the state of the Pipeline has changed'
    //         echo 'For example, if the Pipeline was previously failing but is now successful'
    //     }
    // }
}