
pipeline {
  agent any
  
  stages {

    stage("Prepare Build Environment") {
      
      steps {
          sh 'docker build -t rady-kl .'
      }
    }
    
    stage("run container") {
     
      steps {
           sh 'docker run -it -d --net=host rady-kl'
      }
    }
    
    stage("Bundle AWS Artifact") {
      
      environment {
        appname='rady-kl'
      }
      steps {
        scripts {
          echo "tar -xvzf ${appname}.tgz aws/cf.config aws/cftemplate.json post.bash bootstrap.sh pre.bash"
          echo "make  ${appname}"
          echo "cp ${appname} ${appname}-1.0.0"
        }
      }
    }
    
    stage("Publish AWS Artifact") {
     
      steps {
        scripts {
           echo "curl -LO  myapp.tgz <ArtifactoryURL>"
        }
      }
    }
    
    stage("AWS Deploy") {
      
      steps {
        scripts {
           echo "aws cloudformation create myapp.tgz"
        }
      }
    }
  }
}
