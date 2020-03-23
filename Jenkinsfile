nodejs('Node 10') {
    stages {
        stage('Setup Tools') {
            steps {
                echo 'Installin VBT and SFDX-CLI..'
                sh npm install -g vlocity sfdx-cli
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}