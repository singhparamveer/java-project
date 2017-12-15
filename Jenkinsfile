pipeline {
  agent any

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
     sh 'cp dist/rectangle* /var/www/html/all/'
}
}
 stage ('Test on Debain-Docker'){
       agent {
      docker 'openjdk:8u151-jre'
}
steps  {
  sh  "wget http://18.217.243.103/all/rectangle_${env.BUILD_NUMBER}.jar"
  sh  "sleep 1"
  sh  "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 5"
}
}
stage('Promote To Green'){
  steps {
sh "cp /var/www/html/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/green"
sh "cd /var/www/html/all/;(ls -t|head -n 5;ls)|sort| uniq -u | xargs rm"

}

}
 stage('Promote Development Branch to Master') {
      steps {
        echo "Stashing Any Local Changes"
        sh 'git stash'
        echo "Checking Out Development Branch"
        sh 'git checkout devlopment'
        echo 'Checking Out Master Branch'
        sh 'git pull origin'
        sh 'git checkout master'
        echo 'Merging Development into Master Branch'
        sh 'git merge devlopment'
        echo 'Pushing to Origin Master'
        sh 'git push origin master'
      }
}
}
}
