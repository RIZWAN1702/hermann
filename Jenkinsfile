#!groovy

pipeline {
  agent any
  options {
    // Only keep the 10 most recent builds
    buildDiscarder(logRotator(numToKeepStr:'10'))
  }
  stages {
    stage ('Install') {
      steps {
        // install required bundles
        sh 'bundle install'
      }
    }
    stage ('Build') {
      steps {
        // build
        sh 'bundle exec rake build'

        // Archive the built artifacts
        archive includes: 'pkg/*.gem'
      }
    }
    stage ('Test') {
      steps {
        // run tests with coverage
        sh 'bundle exec rake spec'

        // publish html
        publishHTML target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: true,
            reportDir: 'coverage',
            reportFiles: 'index.html',
            reportName: 'RCov Report'
          ]
      }
    }
  }
}
