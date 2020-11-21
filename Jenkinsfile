pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        timestamps() {
          sh 'pwd'
          sh 'echo $JOB_NAME;echo $JOB_BASE_NAME; echo $WORKSPACE; echo $JENKINS_HOME'
          sh 'cp ${jenkinsHome}/scripts/build.sh ./'
          sh '  sh build.sh $code_env $job_name_new $jenkinsHome $job_name_new'
          sh 'rm build.sh'
        }

      }
    }

    stage('dispatch') {
      steps {
        sh 'pwd'
        sh 'ln=${job_name_new}_${BUILD_NUMBER};sh ${jenkinsHome}/scripts/ndispatch.sh -p $WORKSPACE/jenkins-deploy.tar.gz -i 10.211.55.3 -d /data/application -b /data/appbak -n $ln'
        echo '$JOB_NAME'
        timestamps() {
          sh 'echo \'hello\''
        }

      }
    }

    stage('stop service') {
      steps {
        sh '      ssh -o StrictHostKeyChecking=no deploy@$deploy_ip "sh /data/application/${job_name_new}/service.sh stop"'
      }
    }

  }
  environment {
    j_name = 'jenkins_here'
    jenkinsHome = '/data/application/jenkins-new'
    code_env = 'test'
    job_name_new = 'jenkins-deploy'
  }
}