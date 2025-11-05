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
            archiveArtifacts artifacts: 'calculator/target/site/jacoco/**', fingerprint: true

            jacoco execPattern: 'calculator/target/jacoco.exec',
                   classPattern: 'calculator/target/classes',
                   sourcePattern: 'calculator/src/main/java',
                   exclusionPattern: '**/test/**'
        }

        success {
            emailext(
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Good news! 🎉
                The Jenkins build for *${env.JOB_NAME}* completed successfully.

                📦 Build Number: ${env.BUILD_NUMBER}
                📅 Date: ${new Date()}
                🔗 View build details: ${env.BUILD_URL}
                """,
                to: "seyeolaleye06@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Uh oh 😕
                The Jenkins build for *${env.JOB_NAME}* has failed.

                📦 Build Number: ${env.BUILD_NUMBER}
                📅 Date: ${new Date()}
                🔗 Check the logs here: ${env.BUILD_URL}
                """,
                to: "seyeolaleye06@gmail.com.com"
            )
        }
    }
}
