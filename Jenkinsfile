node {

    stage("Git Clone"){

        git credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/murulii/kubernetespipelineproject'
        
    }

     stage('Gradle Build') {
       sh 'chmod +x gradlew'
       sh './gradlew build'

    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t jhooq-docker-demo .'
        sh 'docker image list'
        sh 'docker tag jhooq-docker-demo murulii/k8project:jhooq-docker-demo'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u murulii -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push  murulii/k8project:jhooq-docker-demo'
    }
    
    stage("kubernetes deployment"){
        sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
    }
} 
