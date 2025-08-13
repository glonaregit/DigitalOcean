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
        
        stage ('parallel Dependency scanning') {
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
                            --prettyPrint''', odcInstallation: 'owasp'

                        dependencyCheckPublisher failedTotalCritical: 6, pattern: 'dependency-check-report.xml', stopBuild: false
                    }
                }
             }
        }
            }
        }
    