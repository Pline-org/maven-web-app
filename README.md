pipeline {
     agent any 
     tools {
       maven "maven3.8.4"
     }
  stages{
    stages('Gitclone') {
        steps{
         sh "echo clone the latest applicatios version"
         git https://github.com/Pline-org/Maven-web-application
        }
    }
    stages('Testbuild'){
      steps{
        sh  "echo Running unitTesting"
        sh  "echo unitTesting ok. Creating packages"
        sh  "mvn clean package"
        sh  "echo Artifacts created"
      }
    }
    stages('codeQuality'){
     steps{
        sh  "echo Running codeQuality Report"
        sh  "mvn sonar:sonar" 
      }
    }
    stages('uploadArtifacts'){
    steps{
        sh  "echo uploadArtifacts into nexus"
        sh  "mvn deploy" 

      }
    }
    stages('message'){
      steps{
        sh  "echo CI job successful"
       }

     }

   }

}

2. Continious Deployment job:
  Deploy to docker 

  pipeline{
    agent any 
    stages{
      stages( 'predeployment'){
        steps{
          sh "docker build"
         // sh "docker login -u mylamndmarktech"
          sh "docker push" 

           }
         }      
          stages( 'deployment'){
        steps{
          sh "echo ready for deployment"
          sh "docker run" 
         }
       }
       }
    }
  }
