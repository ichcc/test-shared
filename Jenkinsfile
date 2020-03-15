#!groovy

def loadValuesYaml(){
  def valuesYaml = readYaml (file: './config.yml')
  return valuesYaml;
}

def transformIntoStep(inputString) {
    // We need to wrap what we return in a Groovy closure, or else it's invoked
    // when this method is called, not when we pass it to parallel.
    // To do this, you need to wrap the code below in { }, and either return
    // that explicitly, or use { -> } syntax.
    def 
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
                    stepsForParallel = valuesYaml.test.collectEntries { 
                            ["${it}" : transformIntoStep(it)]
                        }
                        println stepsForParallel
                        println stepsForParallel.getClass()
                        // stage(stageName) {
                        //     parallel stepsForParallel
                        // }
                      
                //     valuesYaml.test.each{
                        
                //         def taskName = transformIntoStep(it.name)
                //         def task = "${it.testCommand}"
                //         // println it.name
                //         // parallel{ stage(transformIntoStep(it.name)){ task } }
                //         // parallel{
                //         //     stage(taskName){
                //         //         println task
                //         //     }
                //         // }    
                //                 // println it.test.testCommand
                //                 //  dir (it.testFolder){
                //                 //     sh "${it.test.testCommand}"
                //                 // }
                            

                //         }                                               
                    }
                }                
            }
    }
}