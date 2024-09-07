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
                    // Checkout the dev branch for building the application
                    git url: 'https://github.com/Etimdevops/JavaCalculator.git', branch: 'dev', poll: false
                }
            }
        }

        stage('Build (dev)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    // Run the build playbook with inventory from the master branch
                    ansiblePlaybook credentialsId: 'JenkinsAns', 
                                    disableHostKeyChecking: true, 
                                    installation: 'Ansible', 
                                    inventory: '/home/ec2-user/JavaCalculator/hosts.ini', 
                                    playbook: '01-build.yml'
                }
            }
        }

        stage('Checkout for Test (qa)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    // Checkout the qa branch for running tests
                    git url: 'https://github.com/Etimdevops/JavaCalculator.git', branch: 'qa', poll: false
                }
            }
        }

        stage('Run Tests (qa)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    // Run the test playbook with inventory from the master branch
                    ansiblePlaybook credentialsId: 'JenkinsAns', 
                                    disableHostKeyChecking: true, 
                                    installation: 'Ansible', 
                                    inventory: '/home/ec2-user/JavaCalculator/hosts.ini', 
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
