pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // checkout scmGit(branches: [[name: '*/master']], extensions: [],
                // userRemoteConfigs: [[url: 'https://github.com/KAOZUOI/Teedy.git']])
                sh 'mvn -B -X -DskipTests clean package'
            }
        }

        stage('pmd') {
            steps {
                sh 'mvn -U pmd:pmd'
            }
        }
        stage('Generate Javadoc') {
            steps {
                sh 'mvn javadoc:javadoc --fail-never'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/docs-core/target/site/apidocs/', fingerprint: true
                }
            }

        }
        stage('Test') {
            steps {
                sh 'mvn test surefire-report:report'
            }
        }
    }
    post {
            always {
                archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
                archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
                archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
            }
    }
}
