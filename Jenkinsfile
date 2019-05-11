pipeline {
    agent { label 'master' }
    tools {
        maven 'maven'
  }
    stages {
        stage('Build') {
            parallel {
                stage('Build Backend'){
                    steps {
                        dir('backend'){
                            sh 'mvn clean install spotbugs:spotbugs checkstyle:checkstyle deploy'
                        }
                    }
                    post {
                        always {
                            junit 'backend/target/surefire-reports/*.xml'
                            findbugs canComputeNew: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '**/spotbugsXml.xml', unHealthy: ''
                            checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/checkstyle-result.xml', unHealthy: ''
                           // jacoco()
                        }
                    }
                }
                stage('Build Frontend'){
                    steps {
                        dir('frontend'){
                            sh 'npm install --save'
                            sh 'npm install jest-cli --save'
                            sh 'mvn clean install'
                        }
                    }
                }
            }
        }
        stage('Deploy to test'){
            steps {
                dir('deployment'){
                    echo 'Deploying to test'
                    sh 'ansible-playbook -i dev-servers site.yml'
                }
            }
        }
        stage('API tests'){
            steps {
                echo 'Executing API tests'
            }
        }
        stage('Performance tests'){
            steps {
                echo 'Executing Performance tests'
            }
        }
    }
}
