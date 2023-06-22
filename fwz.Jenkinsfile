pipeline{
    agent{
        label 'linux'
    }
    stages{
        stage('Clone repository'){
            steps{
            git credentialsId: 'github_for_nails', url: 'git@github.com:spa086/nails4you.git'
        }
        }
        
        stage('Build Image'){
            steps{
                script{
                dockerImage = docker.build("spaskyson/nails_django:${BUILD_NUMBER}")
        }   
        }
        }
        stage('Push Image'){
            steps{
                script{
                withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
                dockerImage.push()}
        }
        }
        }  
        stage('Deploy'){
            steps{
                sshagent (credentials: ['jensshto55']) {
                    sh ''' 
                        ssh spa@192.168.1.55  docker stop nails4you
                        ssh spa@192.168.1.55  docker run --name nails4you --rm -d -p  8000:8000 spaskyson/nails_django:${BUILD_NUMBER}
                        '''
        }
        }


        }
}
}
