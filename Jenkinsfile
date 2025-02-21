pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17' 
    }

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        JACOCO_REPORT_PATH = '**/target/site/jacoco/index.html'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([$class: 'GitSCM', 
                        branches: [[name: '*/main']], 
                        userRemoteConfigs: [[url: 'https://github.com/dmvbnoob/spring-petclinic.git']]
                    ])
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Run Tests with Code Coverage') {
            steps {
                sh 'mvn test jacoco:report'
            }
        }

        stage('Publish Jacoco Report') {
            steps {
                jacoco(
                    execPattern: '**/target/jacoco.exec',
                    classPattern: '**/target/classes',
                    sourcePattern: '**/src/main/java',
                    exclusionPattern: '**/test/**'
                )
            }
        }
    }

    post {
        success {
            echo "Build and test completed successfully."
        }
        failure {
            echo "Build failed. Check logs."
        }
    }
}
