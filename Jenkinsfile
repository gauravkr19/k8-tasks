// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: gauravkr19/ubuntu-vault
    command:
    - sleep
    args:
    - infinity
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'shell'
        }
    }
    stages {
        stage('shell') {
            steps {
                withVault(configuration: [engineVersion: 2, skipSslVerification: true, timeout: 60, vaultCredentialId: 'df-vault', vaultUrl: 'http://vault.vault.svc.cluster.local:8200'], vaultSecrets: [[engineVersion: 2, path: 'df/dev/mobileapp/appEnv', secretValues: [[vaultKey: 'CLIENT_ID'], [vaultKey: 'CLIENT_SECRET']]]]) {
                sh '''
                echo $CLIENT_ID
                echo $CLIENT_SECRET
                '''
                }
            }
        }
    }
}
