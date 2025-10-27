pipeline {
    agent any

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
                // Navigate to the same folder for running tests
                dir('calculator') {
                    bat "mvn test"
                }
            }
        }
    }
}
