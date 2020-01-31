pipeline {
    tools {
        jdk 'myjava'
        maven 'mymaven'
    }
    agent none
    stages {
        stage('checkout') {
            agent {label 'mac-agent'}
            steps {
                git 'https://github.com/wills-dtb/DevOpsClassCodes.git' 
            }
        }
        stage('compilez') {  
            agent {label 'mac-agent'}
            steps {
                sh 'mvn compile' 
            }
        }
        stage('codeReview') {
            agent any
            steps {
                sh 'mvn pmd:pmd'
            }
            post {
                always {
                    pmd pattern: 'target/pmd.xml'
                }
            }
        }
        stage('unitTest') {
            agent any
            steps {
                sh 'mvn test'
            }
        }
        stage('metricCheck') {
            agent any
            steps {
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
            post {
                always {
                    cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
                }
            }
        }
        stage('package') {
            agent any
            steps {
                sh 'mvn package'
            }
        }
    }
}
