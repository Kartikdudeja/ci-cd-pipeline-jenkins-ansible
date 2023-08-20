# CI/CD Pipeline for Java Application

This Jenkins pipeline automates the build, testing, analysis, and deployment of a Java application. The pipeline utilizes various tools, including Maven, SonarQube, Nexus, and Ansible.

## Prerequisites

- Jenkins server with necessary plugins installed (Pipeline, Nexus, Slack, Ansible, etc.).
- Maven configured in Jenkins with the label "MAVEN3".
- JDK configured in Jenkins with the label "OracleJDK8".
- SonarQube server configured with necessary plugins.
- Nexus repository for artifact storage.
- Ansible for deployment.

## Configuration

1. **Jenkins Configuration:**
   - Create a new pipeline job in Jenkins.
   - In the pipeline configuration, use the provided script in the Jenkinsfile.

2. **Pipeline Environment Variables:**
   - Configure the environment variables in the pipeline.

3. **Nexus and SonarQube Configuration:**
   - Configure Nexus credentials and SonarQube server in Jenkins credentials section.

4. **Ansible Configuration:**
   - Configure Ansible inventory and playbook in the `ansible` directory.
   - Provide necessary credentials for Ansible deployment.
  
## Pipeline Stages

![screenshot](https://github.com/Kartikdudeja/ci-cd-pipeline-jenkins-ansible/blob/main/CI-CD_Pipeline_Java_App_v1.drawio.png)

1. **Build:**
   - Runs `mvn -s settings.xml -DskipTests install` to build the project.
   - Archives generated WAR files on successful build.

2. **Test:**
   - Runs Maven tests using `mvn -s settings.xml test`.

3. **Checkstyle Analysis:**
   - Performs code style and convention analysis using `mvn -s settings.xml checkstyle:checkstyle`.

4. **Sonar Analysis:**
   - Performs in-depth code analysis using SonarQube.
   - Sets up SonarQube environment and runs Sonar Scanner.

5. **Quality Gate:**
   - Waits for the SonarQube Quality Gate to pass.

6. **Upload Artifact:**
   - Uploads built WAR artifact to Nexus repository.

7. **Ansible Deploy to Staging:**
   - Deploys the application to a staging server using Ansible.

8. **Post-Execution Actions:**
   - Sends a Slack notification about the build status.

## Brief about Ansible Deployment Playbooks

Directory Structure Organization:

```
ansible/
├── template/
│   ├── application.j2
│   ├── epel7-svcfile.j2
│   └── ubunutu16-svcfile.j2
├── deploy_app.yml
├── setup_tomcat.yml
├── site.yml
└── stage.inventory
```

- `template/` directory contains jinja template files for tomcat service file and java application.properties file
- `stage.inventory`: Inventory file which has the details about the target host for deployment
- `setup_tomcat.yml`: Playbook to Install and Setup Tomcat on Target host
- `deploy_app.yml`: Playbook to Deploy Artifact

## Conclusion

This pipeline streamlines the CI/CD process for a Java application, from building and testing to analysis and deployment. It ensures code quality, proper testing, and efficient deployment to a staging server.
