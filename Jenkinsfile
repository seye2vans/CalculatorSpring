pipeline {
    agent any

    triggers {
        // Run once every day at 12:25 PM
        cron('25 12 * * *')
    }

    stages {
        stage("Checkout") {
            steps {
                git url: 'https://github.com/seye2vans/CalculatorSpring.git', branch: 'main'
            }
        }

        stage("Compile & Package") {
            steps {
                dir('calculator') {
                    bat "mvn clean package"
                }
            }
        }

        stage("Unit Test") {
            steps {
                dir('calculator') {
                    bat "mvn test"
                }
            }
        }

        stage("Code Coverage") {
            steps {
                dir('calculator') {
                    bat "mvn jacoco:report"
                    bat "mvn verify"
                }
            }
        }
    }

    post {
        always {
            // Archive coverage reports for Jenkins dashboard
            archiveArtifacts artifacts: 'calculator/target/site/jacoco/**', fingerprint: true

            // Publish JaCoCo coverage results
            jacoco execPattern: 'calculator/target/jacoco.exec',
                   classPattern: 'calculator/target/classes',
                   sourcePattern: 'calculator/src/main/java',
                   exclusionPattern: '**/test/**'
        }

        success {
            script {
                emailext(
                    subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: """
                        <h2>🎉 Build Successful!</h2>
                        <p>The Jenkins build for <b>${env.JOB_NAME}</b> completed successfully.</p>

                        <ul>
                            <li><b>Build Number:</b> ${env.BUILD_NUMBER}</li>
                            <li><b>Date:</b> ${new Date()}</li>
                            <li><b>Status:</b> SUCCESS ✅</li>
                            <li><b>Triggered by:</b> ${currentBuild.getBuildCauses()[0].userName ?: "Scheduled Trigger"}</li>
                        </ul>

                        <p>🔗 <a href="${env.BUILD_URL}">View Build Details</a></p>
                    """,
                    to: "seyeolaleye06@gmail.com",
                    mimeType: "text/html"
                )
            }
        }

        failure {
            script {
                emailext(
                    subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: """
                        <h2>⚠️ Build Failed!</h2>
                        <p>The Jenkins build for <b>${env.JOB_NAME}</b> has failed.</p>

                        <ul>
                            <li><b>Build Number:</b> ${env.BUILD_NUMBER}</li>
                            <li><b>Date:</b> ${new Date()}</li>
                            <li><b>Status:</b> FAILURE ❌</li>
                            <li><b>Triggered by:</b> ${currentBuild.getBuildCauses()[0].userName ?: "Scheduled Trigger"}</li>
                        </ul>

                        <p>🔗 <a href="${env.BUILD_URL}">View Build Logs</a></p>
                    """,
                    to: "seyeolaleye06@gmail.com",
                    mimeType: "text/html"
                )
            }
        }
    }
}
