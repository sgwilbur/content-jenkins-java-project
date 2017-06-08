pipeline  {
  agent { label 'master' }
  environment {
      DEPLOY_DIR = "/var/jenkins_home/workspace/buildfiles"
  }
  // TODO: The tool name must be pre-configured in Jenkins under Manage Jenkins → Global Tool Configuration.
  tools {
    ant "ant-1.10.1"
    jdk "sun-jdk-8u31"
  }

  stages {

    stage( 'checkout' ) {
        agent {
          label 'master'
        }
        steps {
          git url: 'https://github.com/nick363501/content-jenkins-java-project/', branch: 'development'
        }
    }

    stage ('build') {
      agent {
       label 'master'
      }
      steps {
        sh 'ant -f build.xml -v'
      }
    }

    stage ('envNames'){
      agent {
         label 'master'
      }
      steps {
        sh "env"
      }
    }

    stage ('deploy') {
      agent {
        label 'master'
      }
      steps {
         sh "mkdir -p ${DEPLOY_DIR}"
         sh "cp ${env.WORKSPACE}/dist/rectangle_${env.BUILD_NUMBER}.jar ${DEPLOY_DIR}"
         //sh "scp ${env.WORKSPACE}/dist/rectangle_${env.BUILD_NUMBER}.jar jenkins@10.150.160.29:/home/jenkins/rectangle_${env.BUILD_NUMBER}.jar"
      }
     }

    stage ('Unit Test') {
      agent {
        label 'master'
        }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }

    stage ("Test on Linux") {
      agent {
        label 'master'
      }
      steps {
        sh "java -jar ${DEPLOY_DIR}/rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage ("Promote to Green") {
      agent {
        label 'master'
      }
      when {
        branch 'development'
      }
      steps {
        sh "mkdir -p ${DEPLOY_DIR}/green"
        sh "cp -rf ${DEPLOY_DIR}/rectangle_${env.BUILD_NUMBER}.jar ${DEPLOY_DIR}/green/rectangle_${env.BUILD_NUMBER}.jar"
      }
    }

   }

}
