pipeline {
    agent any 
	environment {
		HOSTNAME = 'ec2-3-133-83-73.us-east-2.compute.amazonaws.com'
    }
    stages {
        stage('Pull') { 
            steps {
                sh 'ping -c 1  $HOSTNAME'
				sh 'rm -rf JenkinsLab'
				sh 'git clone https://github.com/sisondarrenl/JenkinsLab.git'
				sh 'bash JenkinsLab/hello.sh'
            }
        }
        stage('Connect') { 
            steps {
                sh 'chmod 400 JenkinsLab/temp-kp-ohio.pem'
                sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME whoami' 
            }
        }
        stage('Deploy') { 
            steps {
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME sudo yum install git -y'
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME rm -rf JenkinsLab'
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME git clone https://github.com/sisondarrenl/JenkinsLab.git'
		    		sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME sudo rpm -ev ds_agent'
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME sudo bash JenkinsLab/DeploymentScript/AgentDeploymentScript.sh'
            }
        }
        stage('Test') { 
            steps {
				sh 'echo "Insert test scenario here..."'
            }
        }
        stage('Uninstall') { 
            steps {
                echo "Deploy"
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME sudo yum remove git -y'
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME sudo rpm -ev ds_agent'
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME rm -rf JenkinsLab'
				sh 'ssh -T -i "JenkinsLab/temp-kp-ohio.pem" ec2-user@$HOSTNAME exit'
            }
        }
    }
}
