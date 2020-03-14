#!groovy

def loadValuesYaml(){
  def valuesYaml = readYaml (file: './config.yml')
  return valuesYaml;
}

def makeTest(mapa){
    def folder = "${env.STAGE_NAME}Folder"
    def command = "${env.STAGE_NAME}Command"
    // println folder
    // def folder = ""+test+"Folder"
    // println folder
    // println mapa.get('testFolder')
    dir (mapa."${folder}"){
        sh "${command}"
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
                    valuesYaml.test.each{
                        makeTest(it)                       
                    }
                }                
            }
        }
    }
}