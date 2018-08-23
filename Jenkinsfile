pipeline {
    agent any

    tools {
      nodejs 'NodeJS 4.8.6'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'npm install'
            }
            }

        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'npm test'
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging....'
                sh 'npm run package'
                archiveArtifacts artifacts: '**/distribution/*.zip', fingerprint: true
            }
        stage('Deploy with Ansible') {
            steps {
                echo 'Deploying with Ansible..'
                ansiblePlaybook extras: '-u devops', inventory: 'ansible/environments/dev', playbook: 'ansible/frontend.yml' , extraVars : [ 'frontend_version': 0.7.3, 'frontend_url': 'http://jenkins:8080/job/frontend/lastSuccessfulBuild/artifact/distribution']
            }
        }
        }
    }
}
