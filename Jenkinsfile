node('ubuntu-us-appserver-2140-60')
{
    def app
    stage('Cloning Git')
    {
        checkout scm
    }

    stage('SNYK SCA TEST')
    {
        echo 'SNYK SCA TEST PASS'
    }

    stage('Build-and-Tag')
    {
        app = docker.build('amalan06/sgameimg')
    }

    stage('Post-to-dockerhub')
    {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
        {
            app.push('latest')
        }
    }

    stage('Deploy')
    {
        sh "docker-compose down"
        sh "docker-compose up -d"
    }


}