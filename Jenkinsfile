pipeline {
    agent any

    triggers {
        // Runs every day at 12:25 PM
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
  success {
    emailext subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
             body: "Build succeeded! View at ${env.BUILD_URL}",
             to: "seyeolaleye06@gmail.com"
  }
  failure {
    emailext subject: "❌ FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
             body: "Build failed! Check logs at ${env.BUILD_URL}",
             to: "seyeolaleye06@gmail.com"
  }
}

}
