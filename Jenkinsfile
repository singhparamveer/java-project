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
     sh  'mkdir  /var/www/html/all/${env.BRANCH_NAME}'
     sh 'cp dist/rectangle* /var/www/html/all/${env.BRANCH_NAME}'
}
}
 stage ('Test on Debain-Docker'){
       agent {
      docker 'openjdk:8u151-jre'
}
steps  {
  sh  "wget http://18.217.243.103/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
  sh  "sleep 1"
  sh  "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 5"
}
}
stage('Promote To Green'){
  steps {
sh "cp /var/www/html/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/green"
sh "cd /var/www/html/all/${env.BRANCH_NAME};(ls -t|head -n 5;ls)|sort| uniq -u | xargs rm"

}

}
 stage('Promote Development Branch to Master') {
      when {
        branch 'development'
      }
      steps {
        echo "Stashing Any Local Changes"
        sh 'git stash'
        echo "Checking Out Development Branch"
        sh 'git checkout development'
        echo 'Checking Out Master Branch'
        sh 'git pull origin'
        sh 'git checkout master'
        echo 'Merging Development into Master Branch'
        sh 'git merge development'
        echo 'Pushing to Origin Master'
        sh 'git push origin master'
      }
}
}
}
