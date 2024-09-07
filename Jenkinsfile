pipeline {
    agent { label 'ansible' }

    stages {
        stage('Checkout Master (Playbooks and Inventory)') {
            steps {
                dir('/home/ec2-user/JavaCalculator') {
                    // Checkout the master branch to get the playbooks and hosts.ini
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
                                    playbook: '/home/ec2-user/JavaCalculator/01-build.yml'
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
                                    playbook: '/home/ec2-user/JavaCalculator/02-test.yml'
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
