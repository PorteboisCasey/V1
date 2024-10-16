pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                sh 'git status'
            }
        }

        stage('Check Python and pip') {
            steps {
                // Vérifier les versions de Python et pip
                sh 'python --version'
                sh 'pip --version'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                // Créer et activer un environnement virtuel
                sh 'python -m venv venv'
                sh '. venv/bin/activate'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Installer les dépendances
                sh '. venv/bin/activate && pip install --upgrade pip'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Linting') {
            steps {
                // Exécuter flake8 pour le linting
                sh '. venv/bin/activate && flake8 .'
            }
        }

        stage('Check Code Formatting') {
            steps {
                // Vérifier le formatage du code avec black
                sh '. venv/bin/activate && black --check .'
            }
        }

        stage('Run Static Analysis') {
            steps {
                // Exécuter pylint pour l'analyse statique
                sh '. venv/bin/activate && pylint *.py'
            }
        }

        stage('Run Security Checks') {
            steps {
                // Exécuter bandit pour les vérifications de sécurité
                sh '. venv/bin/activate && bandit -r .'
            }
        }

        stage('Run Tests with Coverage') {
            steps {
                // Exécuter les tests unitaires avec couverture
                sh '. venv/bin/activate && coverage run -m unittest discover'
                sh '. venv/bin/activate && coverage report'
            }
        }
    }

    post {
        always {
            // Nettoyer l'environnement virtuel
            sh 'rm -rf venv'
        }
    }
}