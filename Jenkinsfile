pipeline{
  agent any
  options{
    buildDiscarder(logRotator(numToKeepStr:"2"))
  }
  stages{
    stage("SCM"){
      steps{
          git branch: 'main', url: 'https://github.com/chaan2835/icici-jfrog-artifactory.git'
      }
    }
    stage("Maven-Build"){
      steps{
          sh "mvn clean package"
      }
    }
    stage("Artifact exists"){
      steps{
        echo "checking the file in workspace"
        fileExists '/root/.jenkins/workspace/icici-nexus/target/funds-1.0-SNAPSHOT.war'      
        echo "file exists"
      }
    }
    stage("Nexus Artifact upload"){
      steps{
        echo "Artifact upload start-----------"
        nexusArtifactUploader artifacts: [
                                            [
                                              artifactId: 'funds', 
                                              classifier: '', 
                                              file: '/root/.jenkins/workspace/icici-nexus/target/funds-1.0-SNAPSHOT.war', 
                                              type: 'war'
                                            ]
                                          ], 
                              credentialsId: 'nexus-creds', 
                              groupId: 'icic', 
                              nexusUrl: '65.2.175.145:8081/repository/maven-nexus-artifacts/', 
                              nexusVersion: 'nexus3', 
                              protocol: 'http', 
                              repository: 'maven-nexus-artifacts', 
                              version: '1.0-SNAPSHOT'
        
        echo "Artifact uploaded -------------"
      }
    }
    /*stage("jfrog"){
      steps{
        withCredentials([usernamePassword(credentialsId: 'jfrog-creds', passwordVariable: 'Chandra@2835', usernameVariable: 'jenkins')]) {
             echo "jfrog stage"
             sh "ci/build.sh"
        }
      }
    }
    stage("sonar-code-analysis"){
      steps{
        script{
            withSonarQubeEnv(credentialsId: 'sonar-token') {
              sh "mvn sonar:sonar"
          }
        }
      }
    }
    stage("sonar-quality-check"){
      steps{
        script{
          waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
        }
      }
    }
    stage("Docker-login"){
      steps{
          sh "docker login -u chaan2835 -pChandra@2835"
          sh "docker build -t chaan2835/icici-jfrog-artifactory ."
          sh "docker push chaan2835/icici-jfrog-artifactory"
          }
      }*/
    }
}
