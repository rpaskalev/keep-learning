pipeline {
  agent any
    
  stages {
        
    stage('build image') {
      steps {
        sh 'docker build -t rady-project .'
      }
    }
     
    stage('run container') {
      steps {
         sh 'docker run -it -d --net=host rady-project'
      }
    }      
  }
}
