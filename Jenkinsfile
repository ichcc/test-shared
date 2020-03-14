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
                sh 'mvn --version'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}