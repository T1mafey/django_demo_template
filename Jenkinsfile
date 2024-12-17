pipeline {
    agent any
    environment {
        IMAGE_NAME = "T1mafey/django_demo"
    }
    stages {
        stage("test") {
            steps {
                build job: '/lib/django-test-parametrized',
                    parameters: [
                        string(name: 'GIT_URL', value: "${GIT_URL}"),
                        string(name: 'GIT_BRANCH', value: "${GIT_BRANCH}")
                    ]
            }
        }
        stage("build") {
            steps {
                build job: '/lib/django build parametrized',
                    parameters: [
                        string(name: 'GIT_URL', value: "${GIT_URL}"),
                        string(name: 'GIT_BRANCH', value: "${GIT_BRANCH}"),
                        string(name: 'IMAGE_NAME', value: "${IMAGE_NAME}"),
                        string(name: 'GIT_COMMIT_HASH', value: "${GIT_COMMIT}")
                    ]
            }
        }
        stage("push") {
            steps {
                withCredentials(
                    [
                        usernamePassword(usernameVariable: 'LOGIN', passwordVariable: 'PASSWORD', credentialsId: 'chiv_docker_hub_token')
                    ]
                    ) {
                        sh 'docker login -u ${LOGIN} -p ${PASSWORD}'
                        sh 'docker push ${IMAGE_NAME}:latest'
                    }
            }
        }
    }
}
