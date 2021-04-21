pipeline{
agent any
stages
{
stage("git")
{
steps
{
    git credentialsId: 'github_credentials', url: 'https://github.com/suryambose/CI-Docker-Maven.git'
}
}
stage("SonarQube Analysis")
{
steps
{
    withSonarQubeEnv('sonarserver')
	{
	bat 'mvn clean package sonar:sonar'
	}
}
}
stage('Quality Gate')
{
steps{
waitForQualityGate abortPipeline: true	
}
}
}
}
