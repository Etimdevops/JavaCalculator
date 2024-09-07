pipeline {
    agent {
        label 'ansible'
    }
    environment {
        ANSIBLE_INVENTORY = '/home/ec2-user/JavaCalculator/hosts.ini'
        ANSIBLE_PLAYBOOK_PATH = '/home/ec2-user/JavaCalculator'
    }
    stages {
        stage('Checkout Code for Build') {
            steps {
                script {
                    checkout scm: [
                        $class: 'GitSCM',
                        branches: [[name: 'dev']],
                        userRemoteConfigs: [[url: 'https://github.com/Etimdevops/JavaCalculator.git']]
                    ]
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    dir(ANSIBLE_PLAYBOOK_PATH) {
                        git branch: 'master', url: 'https://github.com/Etimdevops/JavaCalculator.git'
                    }
                    
                    ansiblePlaybook(
                        playbook: "${ANSIBLE_PLAYBOOK_PATH}/01-Build.yml",
                        inventory: "${ANSIBLE_INVENTORY}",
                        credentialsId: 'JenkinsAns'
                    )
                }
            }
        }
        stage('Checkout Code for Test') {
            steps {
                script {
                    checkout scm: [
                        $class: 'GitSCM',
                        branches: [[name: 'qa']],
                        userRemoteConfigs: [[url: 'https://github.com/Etimdevops/JavaCalculator.git']]
                    ]
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dir(ANSIBLE_PLAYBOOK_PATH) {
                        git branch: 'master', url: 'https://github.com/Etimdevops/JavaCalculator.git'
                    }
                    
                    ansiblePlaybook(
                        playbook: "${ANSIBLE_PLAYBOOK_PATH}/02-Test.yml",
                        inventory: "${ANSIBLE_INVENTORY}",
                        credentialsId: 'JenkinsAns'
                    )
                }
            }
        }
    }
    post {
        always {
            script {
                // Print the current working directory
                sh 'pwd'
                // List contents of the test-reports directory for debugging
                sh 'ls -la /tmp/RaviCalculator/test-reports/'
                // List the Jenkins workspace directory for verification
                sh 'ls -la /home/ec2-user/workspace/JenkinsAnsible/'
            }
            // Archive test reports
            archiveArtifacts artifacts: '/tmp/RaviCalculator/test-reports/**', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
