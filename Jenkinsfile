pipeline {
    agent any
    environment {
        NEXUS_URL = '44.202.215.134:8081' // comment
        NEXUS_REPOSITORY = 'sample_java'  // Replace with your repository name
        NEXUS_GROUP_ID = 'org.springframework.boot' // Replace with your group ID
        NEXUS_ARTIFACT_ID = 'spring-boot-starter-parent'          // Replace with your artifact ID
        NEXUS_VERSION = '0.0.1-SNAPSHOT'              // Replace with your version
        CREDENTIALS_ID = 'Nexus3' // The ID of the credentials in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/jayanthmatta/sample_java.git']])
            }
        }
        stage('Maven') {
            steps {
                sh "mvn clean install"
            }
        }
        stage('Sonar') {
            steps {
                sh "mvn sonar:sonar -Dsonar.projectKey=sample_java -Dsonar.host.url=http://54.164.172.98:9000 -Dsonar.login=1eec39150c639c4d5210400a84123301cff584ed"
            }
        }
        stage('Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${NEXUS_URL}",
                    groupId: "${NEXUS_GROUP_ID}",
                    version: "${NEXUS_VERSION}",
                    repository: "${NEXUS_REPOSITORY}",
                    credentialsId: "${CREDENTIALS_ID}",
                    artifacts: [
                        [artifactId: "${NEXUS_ARTIFACT_ID}",
                            classifier: '',
                            file: '/var/lib/jenkins/workspace/CI/target/demo-0.0.1-SNAPSHOT.jar', // Path to your .jar file
                            type: 'jar'
                            
                        ]
                    ]    
                )
            }
        }
    }
}    

