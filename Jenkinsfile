pipeline {
    agent {
        label "jenkins-maven-java11"
    }
    stages {
        stage('Build') {
            steps {
                mavenBuild()
            }
        }
        stage('Sonar Scan') {
            steps {
                sonarScan()
            }
        }
        stage("functional test") {
            steps {
                container('maven') {
                    sh "cd functional-tests && mvn clean test -DappUrl=${APP_URL}"
                }
            }
        }
    }
}

def mavenBuild() {
    container('maven') {
        sh "mvn clean install"
    }
}

def sonarScan() {
    container('maven') {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'sonarqubeToken')]) {
            sh "mvn sonar:sonar -Dsonar.login=${sonarqubeToken}"
        }
    }
}

