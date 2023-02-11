pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '10', numToKeepStr: '7')
    }
    parameters {
        choice choices: ['develop', 'qa', 'master'], description: 'Choose the branch to build', name: 'branchName'
    }
    stages {
        stage ('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage ('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar7') {
                    sh 'mvn sonar:sonar'
                }

                timeout(time: 1, unit: 'HOURS') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'Ok') {
                            error "Pipeline abourted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        } 
    }
}