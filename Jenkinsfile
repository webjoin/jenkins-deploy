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
        sh 'ln=${job_name_new}_${BUILD_NUMBER};sh ${jenkinsHome}/scripts/ndispatch.sh -p $WORKSPACE/jenkins-deploy.tar.gz -i $deploy_ip -d /data/application -b /data/appbak -n $ln'
        echo '$JOB_NAME'
        timestamps() {
          sh 'echo \'hello\''
        }

      }
    }

    stage('stop service') {
      steps {
        sh '      ssh -o StrictHostKeyChecking=no deploy@$deploy_ip "sh /data/application/${job_name_new}/service.sh stop"'
        sh 'sleep 5;/bin/sh ${jenkinsHome}/scripts/check.sh -r 1 -a $deploy_ip -u  http://${deploy_ip}:${deploy_port}${check_path}'
      }
    }

  }
  environment {
    j_name = 'jenkins_here'
    jenkinsHome = '/data/application/jenkins-new'
    code_env = 'test'
    job_name_new = 'jenkins-deploy'
    deploy_ip = '10.211.55.3'
    check_path = '/actuator/info'
    deploy_port = '10091'
  }
}