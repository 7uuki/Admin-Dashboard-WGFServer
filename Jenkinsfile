// Jenkinsfile (Declarative Pipeline)

// Der gesamte Pipeline-Code muss in einem 'pipeline' Block gekapselt sein.
pipeline {
    // Definiere den Agenten, auf dem der Job ausgeführt werden soll.
    // Hier wird ein Docker-Container mit Node.js 18 (alpine-Variante für geringe Größe) als Agent genutzt.
    // Dies stellt sicher, dass 'npm' verfügbar ist.
    agent {
        docker {
            image 'node:18-alpine'
            // Optionale Argumente, z.B. für die Mounts von Docker-Volumes
            // args '-v /your/host/path:/container/path'
        }
    }

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
        // Stufe 1: Installation der npm-Abhängigkeiten
        // Der SCM-Checkout wird automatisch durch den 'agent docker' Block durchgeführt.
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                // Führe 'npm install' aus, um alle Abhängigkeiten zu installieren.
                // Node.js und npm sind im Docker-Image enthalten.
                sh 'npm install'
                echo 'npm dependencies installed.'
            }
        }

        // Stufe 2: Build des Vue.js Projekts
        stage('Build Vue.js Project') {
            steps {
                echo 'Building Vue.js project...'
                // Führe 'npm run build' aus. Dies erstellt normalerweise den 'dist'-Ordner.
                sh 'npm run build'
                echo 'Vue.js project built successfully. Build artifacts are in the "dist" folder.'
            }
        }

        // Stufe 3: Verschieben der gebauten Dateien in das Zielverzeichnis
        stage('Deploy Build Artifacts') {
            steps {
                echo "Deploying build artifacts to ${TARGET_BUILD_DIR}..."
                // Überprüfe, ob der 'dist'-Ordner existiert, bevor er verschoben wird.
                script {
                    // Der 'dist'-Ordner wird direkt im Wurzelverzeichnis des Workspaces erstellt,
                    // da das Git-Repository dorthin geklont wird.
                    def distPath = "dist"
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
