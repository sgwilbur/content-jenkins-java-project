pipeline  {
  agent none
    stages {

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
         sh "cp ${env.WORKSPACE}/dist/rectangle_${env.BUILD_NUMBER}.jar /var/lib/jenkins/buildFiles"
         sh "scp ${env.WORKSPACE}/dist//rectangle_${env.BUILD_NUMBER}.jar jenkins@10.150.160.29:/home/jenkins/rectangle_${env.BUILD_NUMBER}.jar"
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
        sh "java -jar /var/lib/jenkins/buildFiles/rectangle_${env.BUILD_NUMBER}.jar 3 4"
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
        sh "cp -rf /var/lib/jenkins/buildFiles/rectangle_${env.BUILD_NUMBER}.jar /var/lib/jenkins/buildFiles/green/rectangle_${env.BUILD_NUMBER}.jar"
       }
     }
   }
}