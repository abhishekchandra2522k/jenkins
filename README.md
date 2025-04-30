# Jenkins

NOTE: compare the files to see the added/changes in the stages of the Jenkins pipelines. All tests are done on AWS EC2 instances (Jenkins, Nexus, SonarQube EC2 instances)

### Jenkins Tools / Plugins used

- JDK 17
- Maven 3.9.9
- Git
- SonarQube Scanner
- Nexus Artifact Uploader
- Slack Notificaiton

### vprofile_pipeline

    - Fetch Code (Using GIT plugin)
    - Unit Test (`mvn test`)
    - Build (`mvn install -DskipTests`)
    - checkstyle_code_analysis (`mvn checkstyle:checkstyle`)
    - sonarqube_code_analysis (Using SonarQube Scanner Plugin)
    - nexus_upload_artifact (Using Nexus Artifact Uploader Plugin)
    - slack_notification (Using Slack Notification Plugin)
