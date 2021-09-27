
pipeline {
    agent any

    stages {
      stage('Build') { 
        steps {
            sh 'mvn clean install'
        }
      }
      
      stage('Copy artifact to s3') {
        steps {
            sh 'aws s3 cp target/*jar s3://jworks-yp-releases/bas-petclinic.jar'
        }
      }
      stage('Deploy EC2 server') {
        steps {
            sh "aws ec2 run-instances --count 1 --image-id ami-0d1bf5b68307103c2 \
                              --instance-type t2.micro --key-name 'Iebe | RSA' --security-group-ids sg-0d9321c6d92ad3f10 \
                              --subnet-id subnet-5b51263d --associate-public-ip-address \
                              --user-data file://aws/ec2-setup --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=demo-iebe}]' \
                              --iam-instance-profile Name=YPDeployRole --region eu-west-1"

        }
      }
   }
}
