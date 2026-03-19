pipeline {
agent any

environment {
    DOCKER_IMAGE = "rajeshtutta123/longjourneycar_project_img1"
    CONTAINER_NAME = "longjourneycar_cont"
}

stages {

    stage('GIT CHECKOUT') {
        steps {
            git branch: 'main',
            credentialsId: 'rajeshcred',
            url: 'https://github.com/rajeshtutta/longjourneycar.git'
        }
    }

    stage('BUILD') {
        steps {
            sh 'mvn clean package -DskipTests'
        }
    }

    stage('Docker Build') {
        steps {
            sh 'docker build -t $DOCKER_IMAGE:latest .'
        }
    }

    stage('Docker Login') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                sh 'echo $PASS | docker login -u $USER --password-stdin'
            }
        }
    }

    stage('Push Image') {
        steps {
            sh 'docker push $DOCKER_IMAGE:latest'
        }
    }

    stage('Deploy Container') {
        steps {
            sh '''
            docker rm -f $CONTAINER_NAME || true
            docker run -d --name $CONTAINER_NAME -p 1234:80 $DOCKER_IMAGE:latest
            '''
        }
    }

}

}
