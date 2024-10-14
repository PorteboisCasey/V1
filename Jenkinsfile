pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
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
            // Actions à exécuter toujours après les étapes
            echo 'Pipeline terminé.'
        }
        success {
            // Actions à exécuter en cas de succès
            echo 'Pipeline réussi!'
        }
        failure {
            // Actions à exécuter en cas d'échec
            echo 'Pipeline échoué.'
        }
    }
}