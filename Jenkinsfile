pipeline {
    agent {
        label "master"
    }
    tools {
        maven "maven"
    }
	environment {
        // This can be nexus3 or nexus2 server
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "192.168.1.16:8081"
        // Repository where we will upload the artifact
       // NEXUS_REPOSITORY_RELEASES = "maven-releases"
        //NEXUS_REPOSITORY_SNAPSHOTS = "maven-snapshots"
        // Jenkins credential id to  authenticate and connect to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus"
    }
      stages {
        stage("clone code from Git"){
            steps{
               git credentialsId: 'github_credentials', url: 'https://github.com/suryambose/CI-Docker-Maven.git'
               echo 'Clone the code from Github'
            }
        }
		//sonarqube
		stage('SonarQube analysis') {
		steps{
    withSonarQubeEnv(credentialsId: '95ae44b1a71dc1a6355c3d092b7ae182a862a149', installationName: 'sonarserver') { // You can override the credential to be used
      bat 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
    }
  }
  }
	  }
	  }
		
