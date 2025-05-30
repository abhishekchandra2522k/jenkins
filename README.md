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

### Build Triggers

#### Git Webhook

Steps:

1. Repo Settings -> Create GitHub Webhook -> `http://<jenkins-ip>:8080/github-webhook/` (Make sure Jenkins server allows traffic from anywhere on port 8080)
2. Pipeline Configure -> Enable GitHub hook trigge fir GITScm Polling

#### Poll SCM (Jenkins reaches GitHub to check for any commit)

Steps:

1. Enable Poll SCM on pipeline configuration.
2. Define cron job to check the GitHub commit at which interval.

#### Build Periodically

Steps:

1. Enable Build Periodically on pipeline configuration.
2. Define a cron job and the pipeline will trigger at that specific time regularly.
3. Example: `30 20 * * 1-5` triggers pipeline at 8:30PM Monday to Friday. (SEC HR DOM MONTH DOW)

### Jenkins Remote Trigger

1. Create a token `<remote-build-token>` from the pipeline configuration.

##### Job URL

JENKINS_URL/job/build-trigger/build?token=TOKEN_NAME

##### Jenkins USER API Token:

username:token

##### Generating Jenkins CRUMB:

wget -q --auth-no-challenge --user `<username>` --password `<password>` --output-document - ' http://`<jenkins-ip>`:8080/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)

Jenkins-Crumb:`<crumb>`

##### Remotely Pipeline Execution Command:

curl -I -X POST http://username:token@`<jenkins-ip>`:8080/job/build-trigger/build?token=`<token-name>` -H "Jenkins-Crumb:`<crumb>`"

We can run the job using the above URL from anywhere, bash scripts, Python, Ruby, Ansible, any programming language.

### Jenkins Security Authorization
![Jenkins Security Authorization](image.png)
