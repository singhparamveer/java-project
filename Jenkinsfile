pipeline {
  agent any

  environment {
    MAJOR_VERSION = 1
  }
options {
   buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))

}

  stages {

     stage ('Unit Tests'){
    steps {
      sh 'ant -f test.xml -v'
      junit  'reports/result.xml'
}
}
    stage('build') {
      steps {
        sh 'ant -f build.xml -v'
      }

 post  {
   success {
       archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
}
}
  }
  stage ('Deploy on Apache'){
           steps {
     sh 'cp dist/rectangle* /var/www/html/all'
}
}
}
}
