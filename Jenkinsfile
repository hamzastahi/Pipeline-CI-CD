pipeline{
  agent any
  tools{
  maven 'Maven'
  }
  stages{
    stage("build"){
      steps{
        echo 'build the application....'
        sh "mvn install"
        
      }
    }
    stage("test"){
      steps{
                echo 'test the application.....'

      }
    }
    stage("deploy"){
      steps{
                echo 'deploy the application....'

        
      }
    }

  }
}
