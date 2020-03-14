#!groovy

def loadValuesYaml(){
  def valuesYaml = readYaml (file: './config.yml')
  return valuesYaml;
}

pipeline {
    agent any
    tools {
        maven 'apache-maven-3.0.1' 
    }
    stages {
        stage('Preparing') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}