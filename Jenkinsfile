pipeline {
     agent any
     stages {
          stage("Compile") {
               steps {
                    sh "./gradlew compileJava"
               }
          }
          stage("Unit test") {
               steps {
                    sh "./gradlew test"
               }
          }
          stage("Code coverage") {
               steps {
                    sh "./gradlew jacocoTestReport"
                    publishHTML (target: [
                         reportDir: 'build/reports/jacoco/test/html',
                         reportFiles: 'index.html',
                         reportName: "JaCoCo Report"
                    ])
                    sh "./gradlew jacocoTestCoverageVerification"
               }
          }
          stage("Static code analysis") {
               steps {
                    sh "./gradlew checkstyleMain"
                    publishHTML (target: [
                         reportDir: 'build/reports/checkstyle/',
                         reportFiles: 'main.html',
                         reportName: "Checkstyle Report"
                    ])
               }
          }
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }
          stage("Docker build") {
               steps {
                    sh "docker build -t danielgara/calculatorcd ."
               }
          }
          stage("Docker push") {
               environment {
                    DOCKER_USERNAME = credentials("docker-user")
                    DOCKER_PASSWORD = credentials("docker-password")
               }
               steps {
                    sh "docker login --username ${DOCKER_USERNAME} --password ${DOCKER_PASSWORD}"
                    sh "docker push danielgara/calculatorcd"
               }
          }

     }
}