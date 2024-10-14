pipeline {
    agent {
        docker {
            image 'python:3.9' // Utiliser une image Docker officielle de Python
            args '-u root' // Exécuter en tant que root pour installer les dépendances
        }
    }

    stages {
        stage('Checkout') {
            steps {
                // Vérifier le code source depuis le dépôt Git
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Installer les dépendances
                sh 'pip install --upgrade pip'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Linting') {
            steps {
                // Exécuter flake8 pour le linting
                sh 'flake8 .'
            }
        }

        stage('Check Code Formatting') {
            steps {
                // Vérifier le formatage du code avec black
                sh 'black --check .'
            }
        }

        stage('Run Static Analysis') {
            steps {
                // Exécuter pylint pour l'analyse statique
                sh 'pylint *.py'
            }
        }

        stage('Run Security Checks') {
            steps {
                // Exécuter bandit pour les vérifications de sécurité
                sh 'bandit -r .'
            }
        }

        stage('Run Tests with Coverage') {
            steps {
                // Exécuter les tests unitaires avec couverture
                sh 'coverage run -m unittest discover'
                sh 'coverage report'
            }
        }
    }

    post {
        always {
            // Actions à exécuter toujours, par exemple, nettoyer les fichiers temporaires
            echo 'Pipeline terminé.'
        }
        success {
            echo 'Pipeline réussi!'
        }
        failure {
            echo 'Pipeline échoué.'
        }
    }
}