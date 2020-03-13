node {
    stage('Clone sources') {
        git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
    }
    stage('Read config') {
       

        def config_yaml = readYaml file: './config.yml'
        
        print config_yaml.notifications
    }
pipeline {
  agent any
  stages {
    stage(‘Error’) {
      steps {
        error “failure test. It’s work”
      }
    }
    stage(‘ItNotWork’) {
      steps {
        echo “is not pass here”
      }
   }
  }
  post {
    success {
      mail to: team@example.com, subject: ‘The Pipeline success :(‘
    }
  }
}


}