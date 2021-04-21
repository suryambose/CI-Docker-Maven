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
}
}
