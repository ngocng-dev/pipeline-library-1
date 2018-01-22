#!groovy
library identifier: 'lib@master', retriever: modernSCM(
  [$class: 'GitSCMSource',
   remote: 'https://github.com/liatrio/pipeline-library.git')

pipeline {
  agent any
  triggers {
    cron('30 16 * * *')
  }
  environment {
    //Common variables
    buildType= "maven"

    hipchatRoom = ""  /* Mandatory */
    email = ""
    devEnvironment = "" /* Mandatory */
    QAEnvironment = "" /* Mandatory */
    stagingEnvironment = "" /* Mandatory */
    prodEnvironment = "" /* Mandatory */
  }
  tools {
    maven 'maven-3.3.9' //label should match Jenkins Global configuration name.
  }
  stages {
    stage('Maven build') {
      steps {
        /* TODO: Write the functionality for building the maven project.
         * function is at vars/Build.groovy
        */
        build("${buildType}")
      }
    }
    stage('Unit tests and code analysis') {
      steps {
        /* TODO: Write the functionality for running the unit tests for a maven project.
         * functions are at vars/runUnitTests.groovy and vars/codeQuality.groovy
        */
        runUnitTests("${buildType}")
        codeQuality()
      }
    }
    stage('Deploy to dev') {
      steps {
        /* TODO: Write the functionality for deploying the maven app to the passed in environment.
         * function is at vars/Deploy.groovy
        */
        deploy("${buildType}", "${devEnvironment}")
      }
    }
    stage('Dev smoke test') {
      steps {
        /* TODO: Write the functionality for running smoke tests.
         * function is at vars/runSmokeTests.groovy
        */
        runSmokeTests("${buildType}")
      }
    }
    stage('Deploy to QA') {
      steps {
        /* TODO: Write the functionality for deploying the maven app to the passed in environment.
         * function is at vars/Deploy.groovy
        */
        deploy("${buildType}", "${QAEnvironment}")
      }
    }
    stage('QA smoke test') {
      steps {
        /* TODO: Write the functionality for running smoke tests.
         * function is at vars/runSmokeTests.groovy
        */
        runSmokeTests("${buildType}")
        input 'Deploy to staging?'
      }
    }
    stage('Deploy to staging') {
      steps {
        /* TODO: Write the functionality for deploying the maven app to the passed in environment.
         * function is at vars/Deploy.groovy
        */
        deploy("${buildType}", "${stagingEnvironment}")
      }
    }
    stage('Staging smoke test') {
      steps {
        /* TODO: Write the functionality for running smoke tests.
         * function is at vars/runSmokeTests.groovy
        */
        runSmokeTests("${buildType}")
        input 'Deploy to production?'
      }
    }
    stage('Deploy to prod') {
      steps {
        /* TODO: Write the functionality for deploying the maven app to the passed in environment.
         * function is at vars/Deploy.groovy
        */
        deploy("${buildType}", "${prodEnvironment}")
      }
    }
    stage('Prod smoke test') {
      steps {
        /* TODO: Write the functionality for running smoke tests.
         * function is at vars/runSmokeTests.groovy
        */
        runSmokeTests("${buildType}")
      }
    }
  }
  post {
    success {
      notify([hipchat_room: "${env.hipchatRoom}", //Mandatory
      email: "${env.email}", //Optional
      build_status: "SUCCESS" ])
    }
    failure {
      notify([hipchat_room: "${env.hipchatRoom}", //Mandatory
      email: "${env.email}", //Optional
      build_status: "FAILED" ])
    }
  }
}
