pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins-flask-lab'
        CONTAINER_NAME = 'jenkins-flask-app'
        APP_PORT = '8081'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is available in the workspace.'
            }
        }
        stage('Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }
        stage('Test') {
            steps {
                bat 'docker run --rm %IMAGE_NAME%:%BUILD_NUMBER% python -m pytest -v'
            }
        }
        stage('Deploy') {
            steps {
                bat '''
                docker rm -f %CONTAINER_NAME% || echo No previous container
                docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:8000 -e APP_VERSION=%BUILD_NUMBER% %IMAGE_NAME%:%BUILD_NUMBER%
                '''
            }
        }
        stage('Verify') {
            steps {
                powershell '''
                $ok = $false
                for ($i = 0; $i -lt 10; $i++) {
                    try {
                        Invoke-WebRequest -UseBasicParsing http://localhost:$env:APP_PORT/health -ErrorAction Stop | Out-Null
                        $ok = $true; break
                    } catch { Start-Sleep -Seconds 2 }
                }
                if (-not $ok) { exit 1 }
                Invoke-WebRequest -UseBasicParsing http://localhost:$env:APP_PORT/ | Out-Null
                '''
            }
        }
    }

    post {
        success { echo 'Pipeline completed successfully.' }
        failure { echo 'Pipeline failed. Check the stage that reported an error.' }
    }
}