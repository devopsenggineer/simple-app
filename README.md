# simple-app

# Steps for uploading artifact into nexus repository and deploy it tomcat server.

1.  Install jenkins server on any platform like say docker

sudo docker run -d -p 8080:8080 --name jenkins-server jenkins/jenkins

2. Once jenkins server started go to Manage Jenkins --> Manage Plugins & from available plugins tab, select and install "SSH-Agent" plugin as well as 'Nexus Artifact Uploader' & 'Pipeline Utility Steps' plugins.

3. Follow this link to setup nexus repository server 'https://kifarunix.com/install-nexus-repository-manager-on-ubuntu/'

4. Follow this link (https://github.com/devopsenggineer/hello-world-1.git) to setup and configure tomcat server as well as java jdk version 8 on same server or different server.

5. Once nexus server is up and running hit this link (http://nexus-server-ip:8081) & create two repository named as 'simpleapp-snapshot' of type 'snapshot' & 'simpleapp-release' of type 'release'.

6. Now in jenkins server go to Manage-Jenkins-->Manage-Credentials-->click on Global --> Add credentials and give  nexus user credentials (username: admin & password: admin) and give creds  ID as 'nexus3'.

7. Now from jenkins dashboard menu click on 'New Item' --> give some name to job/project like maven-CICD , select 'Pipeline' & click 'Ok'

8. On configure page under 'Build-Triggers' tab/section, select 'Poll SCM' and give appropriate scheduling like (*/10 * * * *) to check for code changes in this git repo every 10 minutes

9. Move to 'Advanced Project Options' tab/section & select "Pipeline script from SCM" in "Definition" field and add this git repo url (https://github.com/devopsenggineer/simple-app.git) and keep everything as same.

Click "Aplly" & "Save".

10. Go to Manage-jenkins-->Global-Tools-Configuration and add a maven installation as "maven 3" and give appropriate path in the path field & click "Apply & Save".

11. Now build the job, once pipeline job is succeeded hit this url "http://tomcat-server-ip:8080/webapp/" to see changes deployed to tomcat server.

12. Also, tomcat server can be checked to view new files added at "/opt/apache-tomcat-8.5.65/webapps/" location using below command

      ls -ltrs /opt/apache-tomcat-8.5.65/webapps/

13. Please also verify/check whether artifact got uploaded into appropriate nexus repositories (simmpleapp-snapshot OR simpleapp-release).

14. Now if you make any changes in this repo and commit code changes, jenkins will trigger CI-CD process.
