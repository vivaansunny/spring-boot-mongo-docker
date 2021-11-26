node {
    stage("Git Clone"){
        git credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/vivaansunny/spring-boot-mongo-docker.git'
    }
    
    stage("MVN build"){
        def mavenHome = tool name: "MAVEN", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn clean package"
        sh mavenCMD
    }
    stage("Docker image creation"){
        sh "docker build -t sunpreet/springboot ."
    }
    stage("Docker push"){
        withCredentials([string(credentialsId: 'DOCKER_CREDENTIALS1', variable: 'password')]) {
        sh "docker login -u sunpreet -p ${password}"
     }
        sh "docker push sunpreet/springboot"
    }
    stage("Kubernetes Deploy"){
        kubernetesDeploy(
            configs: 'springBootMongo.yml',
            kubeconfigId: 'KUBE_ID',
            enableConfigSubstitution: true
            )
    }
}

