pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload')
        {
            steps
            {
                git 'https://github.com/intelliqittrainings/maven.git'
            }
        }
        stage('ContinuousBuild')
        {
            steps
            {
                sh 'mvn package'
            }
        }
        stage('ContinuousDeployment')
        {
            steps
            {
                deploy adapters: [tomcat9(credentialsId: 'e9c1796d-f721-4a0e-917b-20f16f216c85', path: '', url: 'http://172.31.81.214:8080')], contextPath: 'testapp', war: '**/*.war'
            }
        }
        stage('ContinuousTesting')
        {
            steps
            {
                input 'waiting for delivery manager\'s approval'
                git 'https://github.com/intelliqittrainings/FunctionalTesting.git'
                sh 'java -jar /home/ubuntu/.jenkins/workspace/DeclarativePipeline02/testing.jar'
            }
        }
    }
    post
    {
       success 
       {
         deploy adapters: [tomcat9(credentialsId: 'e9c1796d-f721-4a0e-917b-20f16f216c85', path: '', url: 'http://172.31.23.22:8080')], contextPath: 'prodapp', war: '**/*.war'  
       }
       failure
       {
          mail bcc: '', body: 'This job has failed in continuous integration ', cc: '', from: '', replyTo: '', subject: 'Job Failure', to: 'rufusayam@gmail.com'
       }
    }
}
