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
 stage ('Test on Debain-Docker'){
       agent {
      docker 'openjdk:8u151-jre'
}
steps  {
  sh  'sleep 1'
  sh  "wget http://18.217.243.103/all/rectangle_${env.BUILD_NUMBER}.jar"
  sh  'sleep 1'
  sh 'java -jar rectangle*  4 5'
}
}
}
}
