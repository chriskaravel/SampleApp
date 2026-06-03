node {
    def IMAGE_NAME = "sampleapp"
    def IMAGE_TAG = "${BUILD_NUMBER}"
    def DOCKERHUB_USER = "chriskaravel"

    stage('Checkout') {
        git branch: 'master', url: 'https://github.com/chriskaravel/SampleApp.git'
    }

    stage('Restore') {
        sh 'dotnet restore'
    }

    stage('Build') {
        sh 'dotnet build -c Release'
    }

    stage('Publish') {
        sh 'dotnet publish -c Release -o publish'
    }

    stage('Docker Build') {
        sh """
          docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
        """
    }

    stage('Docker Login & Push') {
        withCredentials([usernamePassword(
            credentialsId: '67d62f6c-0658-403c-8d1b-6f43ed4aa934',
            usernameVariable: 'USER',
            passwordVariable: 'PASS'
        )]) {

            sh """
              echo "$PASS" | docker login -u "$USER" --password-stdin

              docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
            """
        }
    }
    stage('Deploy to minikube') {
        sh """
          kubectl set image deployment/sampleapp sampleapp=${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
        """
    }
}
