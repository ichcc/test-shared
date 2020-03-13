node {
    stage('Clone sources') {
        git url: 'https://github.com/glebsamsonov-nbcuni/test-maven-project.git'
    }
    stage('Read config') {
        "pwd".execute().text
        "ls -l".execute().text
        def config_yaml = readYaml file: '../test-maven-project/config.yml'
    }
}