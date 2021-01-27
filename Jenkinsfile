
pipeline {
  agent any
  options { 
    //only keep logs for 5 runs
    buildDiscarder(logRotator(numToKeepStr: '2')) 
    //declarative does a checkout automatically, set this to disable
    
  }

  stages {

    stage("Prepare Build Environment") {
      
      steps {
        scripts {
          sh 'npm install'
        }
      }
    }
    
    stage("Publish Build Environment") {
     
      steps {
        scripts {
           echo "npm publish <Artifactory URL>"
        }
      }
    }
    
    stage("Bundle AWS Artifact") {
      
      environment {
        appname='myapp1'
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
