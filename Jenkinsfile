pipeline {
    agent any
    stages {
        stage('Lint HTML') {
            waitUntil {
                try {
                    steps {
                        sh 'tidy -q -e *.html'
                    }
                } catch(error) {
                    input "Retry the job ?"
                    false
                }
            }
        }

        stage('Upload to AWS') {
            waitUntil {
                try {
                    steps {   
                        withAWS(region:'us-west-2', credentials:'aws-static') {
                            s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'project-jenkins-static-shahmc')
                        }
                    }
                } catch(error) {
                    input "Retry the job ?"
                    false
                }
            }
        }
    }
}