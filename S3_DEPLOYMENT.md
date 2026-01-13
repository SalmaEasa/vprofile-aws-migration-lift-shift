# Artifact Management with Amazon S3

This document outlines the workflow for building the application artifact and storing it in AWS S3 for deployment.

## 1. Build the Artifact
From the project root, use Maven to generate the WAR file:
```bash
mvn clean install -DskipTests
The resulting file is located at: target/vprofile-v2.war

## 2. Upload to S3
Upload the artifact to your private S3 bucket:

Bash

aws s3 cp target/vprofile-v2.war s3://your-vprofile-artifact-bucket/vprofile-v2.war

3. EC2 Retrieval (Bootstrap)
The Tomcat server is configured via User Data to pull this artifact automatically on startup:

Bash

# Example snippet from tomcat_setup.sh
aws s3 cp s3://your-vprofile-artifact-bucket/vprofile-v2.war /usr/local/tomcat/webapps/vproapp.war

Security Note: The EC2 instance must be attached to an IAM Role with `s3:GetObject`
