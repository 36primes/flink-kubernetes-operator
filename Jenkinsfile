pipeline {
    agent any

    stages {
        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker Build&Push') {
            steps {
                sh '''
                    docker build -t fastdata/flink-kubernetes-operator:$BUILD_NUMBER .
                    docker tag fastdata/flink-kubernetes-operator:$BUILD_NUMBER 36primes/fastdata-flink-kubernetes-operator:$BUILD_NUMBER
                    docker push 36primes/fastdata-flink-kubernetes-operator:$BUILD_NUMBER
                '''
            }
        }
        stage('Git Tag') {
            steps {
                sh '''
                    export GIT_SSH_COMMAND="ssh -i ~/.ssh/jenkins_ed25519 -o 'IdentitiesOnly yes'"
                    git config user.name "jenkins"
                    git tag v$BUILD_NUMBER
                    git push origin v$BUILD_NUMBER
                '''
            }
        }

    }
}