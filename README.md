# Jenkins

NOTE: compare the files to see the added/changes in the stages of the Jenkins pipelines. All tests are done on AWS EC2 instances (Jenkins, Nexus, SonarQube EC2 instances)

### Jenkins Tools / Plugins used

- JDK 17
- Maven 3.9.9
- Git
- SonarQube Scanner
- Nexus Artifact Uploader
- Slack Notificaiton

### vprofile_pipeline stages

- Fetch Code (Using GIT plugin)
- Unit Test (`mvn test`)
- Build (`mvn install -DskipTests`)
- checkstyle_code_analysis (`mvn checkstyle:checkstyle`)
- sonarqube_code_analysis (Using SonarQube Scanner Plugin)
- nexus_upload_artifact (Using Nexus Artifact Uploader Plugin) [OPTIONAL - Use AWS ECR]
- docker build
- upload to elastic container service
- slack_notification (Using Slack Notification Plugin) [ALWAYS]

### Jenkins Remote Trigger

Create a token `<remote-build-token>` from the pipeline configuration

##### Job URL

JENKINS_URL/job/build-trigger/build?token=TOKEN_NAME

##### Jenkins USER API Token:

username:token

##### Generating Jenkins CRUMB:

wget -q --auth-no-challenge --user `<username>` --password `<password>` --output-document - ' http://`<jenkins-ip>`:8080/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)

Jenkins-Crumb:`<crumb>`

##### Remotely Pipeline Execution Command:

curl -I -X POST http://username:token@`<jenkins-ip>`:8080/job/build-trigger/build?token=`<token-name>` -H "Jenkins-Crumb:`<crumb>`"
