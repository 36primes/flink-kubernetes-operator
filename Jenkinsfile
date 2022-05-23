/*
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
pipeline {
    agent any

    stages {
        stage('Maven Build') {
            steps {
                sh '''
                    JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
                    mvn --version
                    mvn clean install -DskipTests=true
                '''
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