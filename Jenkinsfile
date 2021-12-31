pipeline {
  agent none
  tools {

  }
  options{
    echo 'OPTIONS goes here'
  }
  stages {
    agent any

    options {
      skipDefaultCheckout()
    }

    stage("Compile") {
      steps {
        sh "./gradlew compileJava --stacktrace"
      }
    }
    stage("Unit test") {
      steps {
        sh "./gradlew test"
      }
    }
    stage("Code Coverage") {
      steps {
        sh "./gradlew jacocoTestReport"
        publishHTML(target: [
          reportDir: 'build/reports/jacoco/test/html',
          reportFiles: 'index.html',
          reportName: 'Jacoco Report',
          reportTitles: 'The Report'
        ])
        sh "./gradlew jacocoTestCoverageVerification"
      }
    }
    stage("Static Code Analysis") {
      steps {
        sh './gradlew checkstyleMain'
         publishHTML(target: [
          reportDir: 'build/reports/checkstyle',
          reportFiles: 'main.html',
          reportName: 'Checkstyle Report'
        ])
      }
    }
  }

}