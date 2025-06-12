// Jenkinsfile (Declarative Pipeline)

// Der gesamte Pipeline-Code muss in einem 'pipeline' Block gekapselt sein.
pipeline {
    // Definiere den Agenten, auf dem der Job ausgeführt werden soll.
    // 'any' bedeutet, dass Jenkins jeden verfügbaren Agenten nutzen kann.
    // Wenn du einen spezifischen Agenten-Label hast (z.B. 'node-js-agent'),
    // kannst du 'agent { label 'node-js-agent' }' verwenden.
    agent any

    // Definiere Umgebungsvariablen, die du im Pipeline-Skript verwenden wirst.
    environment {
        // Die URL deines Git-Repositories.
        // ERSETZE DIES DURCH DIE AKTUELLE REPO-URL
        GIT_REPOSITORY_URL = 'https://github.com/7uuki/Admin-Dashboard-WGFServer'

        // Der Branch, den du bauen möchtest (z.B. 'main', 'develop', 'production').
        // ERSETZE DIES DURCH DEINEN GEWÜNSCHTEN BRANCH
        GIT_BRANCH = 'main'

        // Das Zielverzeichnis auf dem Jenkins-Agenten (oder einem zugänglichen Pfad),
        // wohin die gebauten Dateien verschoben werden sollen.
        // Stelle sicher, dass der Jenkins-Benutzer Schreibrechte für dieses Verzeichnis hat.
        // ERSETZE DIES DURCH DEINEN GEWÜNSCHTEN ZIELORDNER
        TARGET_BUILD_DIR = '/services/admin-dashboard'
    }

    // Die Pipeline-Stufen definieren die einzelnen Schritte des Jobs.
    stages {
        // Stufe 1: Checkout des Quellcodes
        stage('Checkout Source Code') {
            steps {
                echo 'Checking out source code...'
                // Lösche den Arbeitsbereich, um sicherzustellen, dass keine alten Dateien vorhanden sind.
                cleanWs()
                // Klone das Git-Repository.
                git branch: GIT_BRANCH, url: GIT_REPOSITORY_URL
                echo 'Source code checked out successfully.'
            }
        }

        // Stufe 2: Installation der npm-Abhängigkeiten
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                // Führe 'npm install' aus, um alle Abhängigkeiten zu installieren.
                // Stelle sicher, dass Node.js und npm auf dem Jenkins-Agenten installiert und im PATH sind.
                sh 'npm install'
                echo 'npm dependencies installed.'
            }
        }

        // Stufe 3: Build des Vue.js Projekts
        stage('Build Vue.js Project') {
            steps {
                echo 'Building Vue.js project...'
                // Führe 'npm run build' aus. Dies erstellt normalerweise den 'dist'-Ordner.
                sh 'npm run build'
                echo 'Vue.js project built successfully. Build artifacts are in the "dist" folder.'
            }
        }

        // Stufe 4: Verschieben der gebauten Dateien in das Zielverzeichnis
        stage('Deploy Build Artifacts') {
            steps {
                echo "Deploying build artifacts to ${TARGET_BUILD_DIR}..."
                // Überprüfe, ob der 'dist'-Ordner existiert, bevor er verschoben wird.
                script {
                    def distPath = "Admin-Dashboard-WGFServer/dist"
                    if (fileExists(distPath)) {
                        // Erstelle das Zielverzeichnis, falls es noch nicht existiert.
                        sh "mkdir -p ${TARGET_BUILD_DIR}"
                        // Verschiebe alle Inhalte des 'dist'-Ordners in das Zielverzeichnis.
                        // Der '-rf' Parameter stellt sicher, dass vorhandene Inhalte überschrieben werden.
                        sh "cp -rf ${distPath}/. ${TARGET_BUILD_DIR}/"
                        echo "Build artifacts successfully deployed to ${TARGET_BUILD_DIR}."
                    } else {
                        error "ERROR: 'dist' folder not found after build. Deployment failed."
                    }
                }
            }
        }
    }

    // Post-Build Aktionen (optional)
    post {
        // Wird immer ausgeführt, unabhängig vom Erfolg der Stages.
        always {
            echo 'Pipeline finished.'
        }
        // Wird nur ausgeführt, wenn alle Stages erfolgreich waren.
        success {
            echo 'Pipeline completed successfully!'
        }
        // Wird nur ausgeführt, wenn eine Stage fehlgeschlagen ist.
        failure {
            echo 'Pipeline failed!'
            // Optional: Sende eine Benachrichtigung (z.B. E-Mail, Slack)
        }
        // Wird nur ausgeführt, wenn der Job abgebrochen wurde.
        aborted {
            echo 'Pipeline aborted!'
        }
    }
}
