pipeline {
    agent {
        docker {
            image 'maven:3.8.8-eclipse-temurin-17-alpine'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -B -ntp -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'jboss-credentials', usernameVariable: 'JBOSS_USER', passwordVariable: 'JBOSS_PASS')
                ]) {
                    sshagent (credentials: ['centos-private-key']){
                        sh """
                            pwd

                            ls -la
                            
                            scp -o StrictHostKeyChecking=no target/applicationPetstore.war ec2-user@54.69.220.80:/home/ec2-user

                            ssh ec2-user@54.69.220.80 '~/jboss-eap-7.4/bin/jboss-cli.sh --user=$JBOSS_USER --password=$JBOSS_PASS -c --command="undeploy applicationPetstore.war"'
                            
                            ssh ec2-user@54.69.220.80 '~/jboss-eap-7.4/bin/jboss-cli.sh --user=$JBOSS_USER --password=$JBOSS_PASS -c --command="deploy /home/ec2-user/applicationPetstore.war"'

                            ssh ec2-user@54.69.220.80 'rm -f /home/ec2-user/applicationPetstore.war'
                        
                        """
                    }
                }
            }
        }
    }
}