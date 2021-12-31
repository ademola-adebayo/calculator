pipeline {
  agent any
  options {
     skipDefaultCheckout()
  }

  stages {
    stage('Pre') {
     steps {
        script {
          sudo yum -y install https://packages.endpoint.com/rhel/7/os/x86_64/endpoint-repo-1.9-1.x86_64.rpm
          sudo yum install git
        }
     } 
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