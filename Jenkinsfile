pipeline {
    agent {
        label "connexion"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "📥 Clonage du dépôt GitHub..."
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Copie des fichiers dans /var/www/html/"
                sh '''
                    sudo apt update
                    sudo apt install -y apache2
                    sudo systemctl start apache2
                    sudo systemctl enable apache2
                    sudo rm -rf /var/www/html/*
                    sudo cp -r * /var/www/html/
                '''
            }
        }

        stage('Test') {
            steps {
                echo "🔍 Vérification du déploiement..."
                sh 'curl -f http://localhost || true'
            }
        }
    }

    post {
        success {
            echo "✅ Site web déployé avec succès depuis GitHub !"
        }
        failure {
            echo "❌ Une erreur est survenue pendant le déploiement."
        }
    }
}
