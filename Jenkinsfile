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
                sh 'mvn --version'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}