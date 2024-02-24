pipeline {
   agent none
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
       stage('Deploy') {
           agent any
           steps {
               sshagent (credentials: ['centos-private-key']){
                   sh """
                       scp -o StrictHostKeyChecking=no target/applicationPetstore.war ec2-user@34.220.221.160:/home/ec2-user

                       ssh ec2-user@34.220.221.160 '~/jboss-eap-7.4/bin/jboss-cli.sh -c --command="undeploy applicationPetstore.war"'
                      
                       ssh ec2-user@34.220.221.160 '~/jboss-eap-7.4/bin/jboss-cli.sh -c --command="deploy /home/ec2-user/applicationPetstore.war"'

                       ssh ec2-user@34.220.221.160 'rm -f /home/ec2-user/applicationPetstore.war'
                  
                   """
               }
           }
       }
    //    stage('Deploy with Ansible') {
    //        agent {
    //            docker {
    //                image 'ansible/ansible-runner:1.4.7'
    //                args '-u root'
    //            }
    //        }
    //        environment {
    //            ANSIBLE_HOST_KEY_CHECKING = "False"
    //        }
    //        steps {
    //            sshagent (credentials: ['centos-private-key']){
    //                sh 'ansible-playbook -i hosts ansible/deploy_jboss.yml'
    //            }
    //        }
    //    }
   }
}