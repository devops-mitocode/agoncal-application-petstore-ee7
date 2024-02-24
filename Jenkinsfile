pipeline {
    agent none
    environment {
        JBOSS_CREDENTIALS = credentials('jboss-credentials')
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.8.8-eclipse-temurin-17-alpine'
                }
            }            
            steps {
                sh 'mvn clean package -B -ntp -DskipTests'
            }
        }
        stage('Deploy with jboss-cli.sh') {
            agent any
            steps {
                sshagent (credentials: ['centos-private-key']){
                    sh '''
                        pwd
                        ls -la
                        env | sort


                        scp -o StrictHostKeyChecking=no target/applicationPetstore.war ec2-user@35.91.230.131:/home/ec2-user
                        #ssh ec2-user@35.91.230.131 "~/jboss-eap-7.4/bin/jboss-cli.sh --user=$JBOSS_CREDENTIALS_USR --password=$JBOSS_CREDENTIALS_PSW -c --command='undeploy applicationPetstore.war'"
                        ssh ec2-user@35.91.230.131 "~/jboss-eap-7.4/bin/jboss-cli.sh --user=$JBOSS_CREDENTIALS_USR --password=$JBOSS_CREDENTIALS_PSW -c --command='deploy /home/ec2-user/applicationPetstore.war'"
                        ssh ec2-user@35.91.230.131 'rm -f /home/ec2-user/applicationPetstore.war'
                    '''
                }
            }
        }       
    }
}