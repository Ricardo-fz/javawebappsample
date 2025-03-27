pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID = '880680d2-eb30-43fd-b0ac-182a0895c2bc'
        AZURE_TENANT_ID = 'a576191b-c45b-4921-972e-159cc6de691a'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'AzureServicePrincipal', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
                    sh '''
                        az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID
                        az webapp deploy --resource-group jenkins-get-started-rg --name MTG --src-path target/calculator-1.0.war --type war
                    '''
                }
            }
        }
    }
}

