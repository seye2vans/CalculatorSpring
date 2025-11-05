pipeline {
    agent any

    triggers {
        // Run every 5 minutes
        cron('H/5 * * * *')
    }

    stages {
        stage("Checkout") {
            steps {
                git url: 'https://github.com/seye2vans/CalculatorSpring.git', branch: 'main'
            }
        }

        stage("Compile & Package") {
            steps {
                // Navigate to folder containing pom.xml
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
                    // Generate JaCoCo report and verify thresholds
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

            // Publish JaCoCo coverage in Jenkins (if plugin installed)
            jacoco execPattern: 'calculator/target/jacoco.exec',
                   classPattern: 'calculator/target/classes',
                   sourcePattern: 'calculator/src/main/java',
                   exclusionPattern: '**/test/**'
        }
    }
}
