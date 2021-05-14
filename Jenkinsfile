pipeline {
    agent any
    tools {
        maven 'Maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
                 sh 'cp target/simple-app-1.0.0.war target/webapp.war'
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: 'in.javahome', 
                    nexusUrl: '192.168.0.3:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepoName, 
                    //repository: 'simpleapp-release',
                    version: "${mavenPom.version}"
                    sh 'pwd'
                    sh 'echo Reponame is $nexusRepoName'
                    sh 'wget --user=admin --password=admin http://192.168.0.3:8081/repository/simpleapp-snapshot/in/javahome/simple-app/3.0.0-SNAPSHOT/simple-app-3.0.0-20210513.143540-1.war -O webapp.war'
                }
            }
        }
        stage("deploy"){
            steps{
                sshagent(['tomcat-server-private-key-ID']) {
                    sh "scp -o StrictHostKeyChecking=no webapp.war osboxes@192.168.0.3:/home/osboxes/apache-tomcat-8.5.65/webapps"
                    sh 'rm -rf webapp.war'                 
                }
            }
        }

    }
}
