pipeline {
    agent any
    tools{
        jdk 'OpenJDK'
        maven 'Maven'
    } 
    stages {
        stage('SCM') {
            steps {
              git branch: 'main', changelog: false, poll: false, url: 'https://github.com/animshamura/springboot-devops-integration.git'
            }
        }
    
        stage('Maven Build') {
            steps {
                sh "mvn clean install"
            }
        }
    
        stage('Docker Build & Push') {
            steps {
                script{
                   withDockerRegistry(credentialsId: 'pass') {
                    sh "docker build -t animshamura/app:6609 ."
                    sh "docker push animshamura/app:6609"
} 
                }
            }
}
        stage('Deploy to Kubernetes'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }

}
}
