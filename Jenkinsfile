node {
    checkout scm

    docker.withRegistry('http://192.168.6.181:8082','docker-deployer') {
        docker.image('maven:3-alpine').withRun('-v /root/.m2:/root/.m2') {
            stages {
                stage('Build') {
                    steps {
                        sh 'mvn -B -DskipTests clean package -s ./jenkins/scripts/settings.xml'
                    }
                }
                stage('Test') {
                    steps {
                        sh 'mvn test'
                    }
                    post {
                        always {
                            junit 'target/surefire-reports/*.xml'
                        }
                    }
                }
                stage('Deliver') {
                    steps {
                        sh './jenkins/scripts/deliver.sh'
                    }
                }
            }
        }
    }
}