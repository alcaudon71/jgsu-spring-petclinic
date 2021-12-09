pipeline {
    agent any

    triggers{ pollSCM('H/5 * * * *') }

    //tools {
        // Install the Maven version configured as &quot;M3&quot; and add it to the path.
    //    maven &quot;M3&quot;
    //}

    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/alcaudon71/jgsu-spring-petclinic.git'
            }    
        }    
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                //git branch: &apos;main&apos;, url: &apos;https://github.com/alcaudon71/jgsu-spring-petclinic.git&apos;

                // Run Maven on a Unix agent.
                //sh &quot;mvn -Dmaven.test.failure.ignore=true clean package&quot;
                sh "mvn install"
                //sh "false"
                //sh &quot;./mvnw clean package&quot;

                // To run Maven on a Windows agent, use
                // bat &quot;mvn -Dmaven.test.failure.ignore=true clean package&quot;
            }


        }
    }

                post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                always {
                    junit '**/target/surefire-reports/TEST-*.xml';
                    archiveArtifacts 'target/*.jar';
                }

                changed {
                    emailext attachLog: true, 
                    body: 'Please go to ${BUILD_URL} and verify the build', 
                    compressLog: true, 
                    recipientProviders: [requestor(), upstreamDevelopers()],
                    subject: "Job \'${JOB_NAME}\' (${BUILD_NUMBER}) ${currentBuild.result}", 
                    to: 'alcaudon71@gmail.com'
                }
            }
            
}
