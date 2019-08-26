pipeline {
    agent any
    parameters{
        string(name:'tomcat_dev', defaultValue:'18.232.106.248', description:'Staging Server')
        string(name:'tomcat_prod', defaultValue:'3.82.55.51', description:'Prodcution Server')
    }

    triggers{

        pollSCM('* * * * *')
    }
    stages {
        stage('Build') {
            steps {
               
                        bat 'mvn clean package'
                 
            }
            post{
                success{
                    echo 'Now archiving...'
                   
                    archiveArtifacts '**/target/*.war'
                }
            }

        }
        stage('Deployments') {
            parallel{
                stage('Deployment to Staging')
                {
                    steps {
                        bat "scp **/target/*.war C:\\Users\\91956\\Downloads"
                    }
                }
                 stage('Deployment to Production')
                {
                    steps {
                        bat "scp -i C:\\Users\\91956\\Downloads\\tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }

            }
        }

        
        }
}
