node {
    stage('Clone sources') {
        git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
    }
    stage('Read config') {
       

        def config_yaml = readYaml file: './config.yml'
        
        print config_yaml.notifications
    }
    post {
    always {
      echo 'I will always execute this!'
    }
}
}