pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        timestamps() {
          sh './gradlew build '
        }

      }
    }

    stage('dispatch') {
      steps {
        sh 'pwd'
        sh 'sh ${jenkinsHome}/scripts/ndispatch.sh -p $deploy_gz -i $deploy_ip -d /data/application -b /data/appbak -n $ln'
        echo '$JOB_NAME'
        timestamps() {
          sh 'echo \'hello\''
        }

      }
    }

    stage('stop service') {
      steps {
        sh '      ssh -o StrictHostKeyChecking=no deploy@$deploy_ip "sh /data/application/${prj_name}/service.sh stop"'
      }
    }

  }
  environment {
    j_name = 'jenkins_here'
    jenkinsHome = '/data/application/jenkins-new'
    deploy_gz = ''
  }
}