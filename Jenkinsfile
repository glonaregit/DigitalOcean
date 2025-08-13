pipeline {
    agent any

    tools {
        nodejs 'nodejs-24-5-0'
    }

    stages {
        stage ('Installing Dependencies') {
            steps {
                sh 'npm install --no-audit'
            }
        }

        stage ('Dependency Scanning') {
            parallel {
                stage('NPM Audit') {
                    steps {
                        sh '''
                            npm audit --audit-level=critical
                            echo $?
                        '''
                    }
                }
                stage('OWASP Dependency Check') {
                    steps {
                        dependencyCheck additionalArguments: '''
                            --scan './' 
                            --out './'  
                            --format 'ALL' 
                            --disableYarnAudit \\
                            --prettyPrint''', odcInstallation: 'OWASP'

                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: false
                    }
                }
            }
        }
    }
}