pipeline {
    agent { label 'ansible' }

    stages {
        stage('Checkout Master (Playbooks)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    git url: 'https://github.com/Etimdevops/JavaCalculator.git', branch: 'master'
                }
            }
        }

        stage('Checkout for Build (dev)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    git url: 'https://github.com/Etimdevops/JavaCalculator.git', branch: 'dev'
                }
            }
        }

        stage('Build (dev)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    ansiblePlaybook credentialsId: 'JenkinsAns', 
                                    disableHostKeyChecking: true, 
                                    installation: 'Ansible', 
                                    inventory: 'hosts.ini', 
                                    playbook: '01-build.yml'
                }
            }
        }

        stage('Checkout for Test (qa)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    git url: 'https://github.com/Etimdevops/JavaCalculator.git', branch: 'qa'
                }
            }
        }

        stage('Run Tests (qa)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    ansiblePlaybook credentialsId: 'JenkinsAns', 
                                    disableHostKeyChecking: true, 
                                    installation: 'Ansible', 
                                    inventory: 'hosts.ini', 
                                    playbook: '02-test.yml'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Test stages completed successfully.'
        }
        failure {
            echo 'Build or Test stage failed.'
        }
    }
}
