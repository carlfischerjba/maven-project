pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: '52.209.221.57', description: 'Staging Server IP')
        string(name: 'tomcat_dev_hostkey', defaultValue: 'a6:3e:ee:eb:86:b5:1f:5e:2d:92:10:60:ea:03:a8:41', description: 'Staging Server Host Key')
        string(name: 'tomcat_prod', defaultValue: '18.202.212.170', description: 'Production Server IP')
        string(name: 'tomcat_prod_hostkey', defaultValue: 'c7:f4:ab:de:3d:d5:36:db:67:a3:9d:15:ce:46:e3:13', description: 'Staging Server Host Key')
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
                        bat "\"C:/Program Files/PuTTY/pscp.exe\" -batch -hostkey ${params.tomcat_dev_hostkey} -i C:/Users/carlfischer/Documents/jenkins-tutorial/tomcat-demo.ppk webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "\"C:/Program Files/PuTTY/pscp.exe\" -hostkey ${params.tomcat_prod_hostkey} -batch -i C:/Users/carlfischer/Documents/jenkins-tutorial/tomcat-demo.ppk webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}