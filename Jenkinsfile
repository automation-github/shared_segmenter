pipeline {
  agent any
  environment {
    target_cluster = '10.65.182.11'
  }
  stages {
    stage('installation') {
      steps {
            sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
          }
    }

    stage('Shared_Segmenter_Disconnect_OFF_5.1_182_11') {
      agent any
      steps {
        build (job: 'Shared_Segmenter_Disconnect_OFF_5.1_182_11', propagate: false)
      }
    }

    stage('Shared_Segmenter_Segment_Compare_OFF_5.1_182_11') {
      agent any
      steps {
        build (job: 'Shared_Segmenter_Segment_Compare_OFF_5.1_182_11', propagate: false)
      }
    }

    stage('copy xmls') {
      steps {
        sh '''scp -p root@$target_cluster:/tmp/Shared_Segmenter/*.xml .'''
      }
    }

    stage('check_cores') {
      steps {
        sh 'ssh root@$target_cluster "python2.7 /var/Nightswatch/deploy/find_cores.py"'
      }
    }

    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  }
}
