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
		stage('Sonarqube') {
    environment {
        scannerHome = tool 'sonarserver'
    }
    steps {
        withSonarQubeEnv('sonarserver') {
            bat "${scannerHome}//bin"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
	  }
	  }
		
