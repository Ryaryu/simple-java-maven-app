stage 'Preparando Ambiente'
node {
    sh 'docker login -u mvn-deployer -p spiderman 192.168.6.181:8082'
    sh 'docker login -u mvn-deployer -p spiderman 192.168.6.181:8083'
}

stage 'Gerando Imagem Maven'
node {
    docker.image('192.168.6.181:8082/maven:3-alpine').inside('-v /root/.m2:/root/.m2') {
        stage('Construindo') {
            sh 'mvn -B -DskipTests clean package -s ./jenkins/scripts/settings.xml'
        }
        stage('Testando') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Deployando') {
            sh './jenkins/scripts/deliver.sh'
        }
    }
}
//node {
//    checkout scm
//
//    docker.withRegistry('http://192.168.6.181:8082','docker-deployer') {
//        docker.image('maven:3-alpine').withRun('-v /root/.m2:/root/.m2').inside {
//            stage('Build') {
//                sh 'mvn -B -DskipTests clean package -s ./jenkins/scripts/settings.xml'
//            }
//            stage('Test') {
//                sh 'mvn test'
//                post {
//                    always {
//                        junit 'target/surefire-reports/*.xml'
//                    }
//                }
//            }
//            stage('Deliver') {
//                sh './jenkins/scripts/deliver.sh'
//            }
//        }
//    }
//}