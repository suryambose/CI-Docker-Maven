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
               git credentialsId: 'Gitlab', url: 'https://github.com/suryambose/CI-Docker-Maven.git'
              echo 'Clone the code from Github'
            }
        }
		stage("Quality Gate Status Check")
		{
		steps
		{
		script{
		withSonarQubeEnv(credentialsId: 'sonar-api-key')
		{
		bat "mvn sonar:sonar"
		}
		timeout(time:1,unit:'HOURS')
		{
		def qg=waitForQualityGate()
		if(qg.status !='OK')
		{
		error "Pipeline aborted due to quality gate failure: (qg.status)"
		}
		}
		bat "mvn clean install"
		}
		}
		}
		}	
        stage("Nexus Repository") {
            steps {
                script {
                    def pom = readMavenPom file: "pom.xml";
					//newly added
					def nexusRepoName=pom.version.endsWith("SNAPSHOT") ? "maven-snapshots" : "maven-releases"
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                                nexusVersion: NEXUS_VERSION,
                                protocol: NEXUS_PROTOCOL,
                                nexusUrl: NEXUS_URL,
                                groupId: pom.groupId,
                                version: pom.version,
                               // repository: NEXUS_REPOSITORY_RELEASES,
								repository: nexusRepoName,
                                credentialsId: NEXUS_CREDENTIAL_ID, 
                                artifacts: [
                                    [artifactId: pom.artifactId, 
                                     classifier: '',
                                     file: artifactPath,
                                     type: pom.packaging],
                                    [artifactId: pom.artifactId,
                                     classifier: '',
                                     file: "pom.xml", 
                                     type: "pom"]]);	
                      
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
  }
