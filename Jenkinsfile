#!groovy

def loadValuesYaml(){
  def valuesYaml = readYaml (file: './config.yml')
  return valuesYaml;
}

def makeTest(mapa){
    println mapa.testFolder
    println mapa.name
    println mapa.testCommand

    // mapa.each{
    //     if it.getKey()="testFolder"
    //     println it
    // }

}

pipeline {
    agent any
    stages {
        stage('Preparing') {
            steps {
                git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
                 script{
                    valuesYaml = loadValuesYaml()
                    }
            }
        }
        stage('Build') {
            steps {
                script{
                    dir (valuesYaml.build.projectFolder){
                        sh "${valuesYaml.build.buildCommand}"
                    }
                }
            }
        }
        stage('Database') {
            steps {
                script{
                    dir (valuesYaml.database.databaseFolder){
                        sh "${valuesYaml.database.databaseCommand}"
                    }
                }
            }
        }
        stage('Test') {
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