
pipeline {
  agent any
  options { 
    //only keep logs for 5 runs
    buildDiscarder(logRotator(numToKeepStr: '2')) 
    //declarative does a checkout automatically, set this to disable
    
  }
  environment {
    buildtype='package'
  }
  parameters {
        choice(name: 'Branch', choices: 'master')
  }
  stages {

    stage("Prepare Build Environment") {
      agent { label "master" }
      steps {
        scripts {
          sh 'npm install'
        }
      }
    }
    
    stage("Publish Build Environment") {
      agent { label "master" }
      steps {
        scripts {
           echo "npm publish <Artifactory URL>"
        }
      }
    }
    
    stage("Bundle AWS Artifact") {
      agent { label "master" }
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
      agent { label "master" }
      steps {
        scripts {
           echo "curl -LO  myapp.tgz <ArtifactoryURL>"
        }
      }
    }
    
    stage("AWS Deploy") {
      agent { label "master" }
      steps {
        scripts {
           echo "aws cloudformation create myapp.tgz"
        }
      }
    }
  }
}
