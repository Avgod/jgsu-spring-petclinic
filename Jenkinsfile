properties([pipelineTriggers([githubPush()])])
pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
            }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                // Run Maven on a Unix agent.
                sh "mvn clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('docker build'){
           steps {
            script{
            sh 'docker build -t petclinic:$BUILD_NUMBER .'
                }
            }   
        }
        stage('docker push'){
           steps {
            script{
            sh 'docker tag petclinic:$BUILD_NUMBER Avgod/petclinic:$BUILD_NUMBER'
            sh 'docker push Avgod/petclinic:$BUILD_NUMBER'
             }
            }   
        }
          
//             post {
//                 // If Maven was able to run the tests, even if some of the test
//                 // failed, record the test results and archive the jar file.
//                 success {
//                     junit '**/target/surefire-reports/TEST-*.xml'
//                     archiveArtifacts 'target/*.jar'
//                 }
//             }
        stage('Sonarqube') {
            steps {
                sh "mvn sonar:sonar \
                     -Dsonar.projectKey=java \
                     -Dsonar.host.url=http://43.205.7.80:9000 \
                     -Dsonar.login=7d29c5278f5aaf087726e9ca0367ef8e5c28dbec"
            }
        }
    }
}
