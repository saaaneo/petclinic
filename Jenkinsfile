pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/ravdy/petclinic'

                // Run Maven on a Unix agent.
                sh "mvn -f pom.xml checkstyle:checkstyle findbugs:findbugs pmd:pmd"

                    }
                }
        stage('Archive atifacts'){
            steps {
                archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
            }
    	}
    	stage('Deploy To Tomcat'){
            steps {
        	sshagent(credentials: ['ubuntu_user']) {
             	sh "scp -o StrictHostKeyChecking=no target/sparkjava-hello-world-1.0.war ubuntu@3.92.2.158:/var/lib/tomcat9/webapps/"
                }
            }
        }
        stage('Post Results') {
         post {
            	always {
           		recordIssues(enabledForFailure: true, aggregatingResults: true, 
               	tools: [java(), checkStyle(pattern: 'checkstyle-result.xml', reportEncoding: 'UTF-8'), findBugs(pattern: 'findbugs.xml')]
           		)               
        	    }
            }
        }
    }
}
