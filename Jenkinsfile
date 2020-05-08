pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Security Scan') {
              steps { 
                 aquaMicroscanner imageName: 'alpine', notCompliesCmd: '', onDisallowed: 'ignore', outputFormat: 'html'
              }
         }         
         stage('Upload to AWS') {
              steps {
                  withCredentials([[$class:'AmazonWebServicesCredentialsBinding',accessKeyVariable:'AWS_ACCESS_KEY_ID',credentialsId:'blueocean',secretKeyVariable:'AWS_SECRET_ACCESS_KEY']]) {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'babyd-static-jenkins-pipeline')
                  }
              }
         }
     }
}
