pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {

    }
    parameters {
        choice choices: ['develop', 'qa', 'master'], description: 'choose the branch to build' name: 'branchName'
    }
    stages {
        stage ('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}