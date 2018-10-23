pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: '52.209.221.57', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.202.212.170', description: 'Production Server')
    }
    
    triggers {
        pollSCM('* * * * *')
    }
    
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat '"C:/Program Files/PuTTY/pscp.exe" -i "C:/Users/carlfischer/Documents/jenkins-tutorial/tomcat-demo.ppk" **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat '"C:/Program Files/PuTTY/pscp.exe" -i "C:/Users/carlfischer/Documents/jenkins-tutorial/tomcat-demo.ppk" **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps'
                    }
                }
            }
        }
    }
}