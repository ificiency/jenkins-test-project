pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo "📥 Checking out code from GitHub..."
                // Git will be automatically checked out by Jenkins
            }
        }
        
        stage('Build Info') {
            steps {
                echo "🚀 Build started by: ${currentBuild.getBuildCauses().toString()}"
                script {
                    def causes = currentBuild.getBuildCauses()
                    echo "📋 Detailed causes:"
                    causes.each { cause ->
                        echo "   - ${cause.toString()}"
                    }
                }
                echo "📅 Build time: ${new Date().toString()}"
            }
        }
        
        stage('Execute Build') {
            steps {
                sh '''
                    echo "📂 Workspace contents:"
                    ls -la
                    echo "---"
                    echo "🔍 Checking if build.sh exists..."
                    if [ -f "build.sh" ]; then
                        chmod +x build.sh
                        ./build.sh
                    else
                        echo "❌ build.sh not found!"
                        echo "📁 Current files:"
                        ls -la
                    fi
                '''
            }
        }
        
        stage('Notify') {
            steps {
                echo "✅ Build completed successfully!"
                echo "🌐 Repository URL: ${env.GIT_URL}"
                echo "📝 Commit: ${env.GIT_COMMIT}"
                echo "🎯 Branch: ${env.GIT_BRANCH}"
            }
        }
    }
    
    post {
        always {
            echo "🎉 Pipeline finished with status: ${currentBuild.result}"
            echo "⏰ Duration: ${currentBuild.durationString}"
        }
        success {
            echo "🥳 SUCCESS! Build triggered automatically by GitHub!"
        }
        failure {
            echo "😞 BUILD FAILED - Check the logs above"
        }
    }
}
