<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Continuous Integration with CodeBuild

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codebuild-updated)

**Author:** Nii OB 
**Email:** davidniiamui@gmail.com 

---

![Image](http://learn.nextwork.org/genuine_navy_mysterious_monkey/uploads/aws-devops-codebuild-updated_35588a47)

---

## Introducing Today's Project!

In this project, I will demonstrate how to use AWS CodeBuild to automate the build process in the CI/CD pipeline.
Building means compressing the web app's files into a single compressed file (like a zip file) that can later be deployed.
This project will only focus on CodeBuild and creating that compressed file automatically.
Deployment will come in the next project.

### Key tools and concepts

Services I used were CodeBuild, CodeConnections, CodeArtifact, EC2, S3, GitHub, and VSCode. 
Key concepts I learnt include the build process, resolving Git issues in the terminal, buildspec.yml and using CodeConnections to connect AWS with GitHub.

### Project reflection

This project took me approximately 3 hours, including troubleshooting time and documentation.
It was most rewarding to see the build success at the end.

This project is part four of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project tomorrow. :)

---

## Setting up a CodeBuild Project

CodeBuild is a continuous integration (CI) service, which means it  helps compile and package code (turn a web app's code into something computers can run, then compress it into a neat little package).
Engineering teams use it because CI services automate this process.
If CI didn't exist, engineers would have to manually compile the code themselves before deployment.

My CodeBuild project's source configuration means where the code lives, and I selected GitHub.
That's my web app's source code repository

![Image](http://learn.nextwork.org/genuine_navy_mysterious_monkey/uploads/aws-devops-codebuild-updated_fewgrhte)

---

## Connecting CodeBuild with GitHub

There are multiple credential types for GitHub, like GitHub App, Personal Access tokens and OAuth app.
I used GitHub App because it's the most simple and secure way to set up a connection between GitHub and AWS - AWS handles all of the credentials in the background for me.
I simply need to log in once to GitHub using a special AWS managed GitHubApp.

The service that helped connect my AWS environment with GitHub is AWS CodeConnections.
CodeConnections also allows me to connect my environment with other 3rd party platforms.
In this case, it made sure that I had a secure connection to GitHub so that my CodeBuild project could use my GitHub repository as the source with the correct permissions.

![Image](http://learn.nextwork.org/genuine_navy_mysterious_monkey/uploads/aws-devops-codebuild-updated_a7c98e2d)

---

## CodeBuild Configurations

### Environment

My CodeBuild project's Environment configuration means the environment set up for the compute power (i.e. the EC2 instance that will be compiling and compressing the files when I run the CodeBuild project.
It includes settings like provisioning model, compute operating system, image and service role.
These settings determine how a new EC2 instance will be spun up and conduct the build process.
It's important here that the EC2 instance has correct permissions to everything it needs to work properly.

### Artifacts

Build artifacts are files/resources that get created as part of the CodeBuild build process.
They're important because they represent artifacts that need to be used later on in the CI/CD pipeline. 
For example, my build process will create a compressed file that represents my web app. I'll need to use that compressed to deploy the web app later on. 
To store the build artifacts, I created an S3 Artifact as well.

### Packaging

When setting up CodeBuild, I also chose to package artifacts into a zip file.
This helps with package management as files are all organized neatly in a single executable file.
It also helps to compress the size of the compiled web app.

### Monitoring

For monitoring, I enabled CloudWatch Logs, which is a service that will note down commands that are run and any errors that happen. 
This is very helpful for troubleshooting.


---

## buildspec.yml

My first build failed because CodeBuild could not find the buildspec.yml file in my source code root directory.
A buildspec.yml file is needed because it tells CodeBuild how to run the process.

The first two phases in my buildspec.yml file install Java and get me access to CodeArtifact.
The third phase in my buildspec.yml file compiles the web app.
The fourth phase in my buildspec.yml file packages the web app into a single compressed artifact.

![Image](http://learn.nextwork.org/genuine_navy_mysterious_monkey/uploads/aws-devops-codebuild-updated_35588a47)

---

## Success!

My second build also failed, but with a different error that said I failed to run an error while executing the command to compile the web app 'mvn -s settings.xml compile'.
To fix this, I need to add a new policy to CodeBuild that grants CodeArtifact permissions.

To resolve the second error, I attached a policy for accessing CodeArtifact to my CodeBuild role.
When I built my project again, I saw "Succeeded" status.
With the privilege to CodeArtifact granted by the role, CodeBuild now has access to the repository.

To verify the build, I checked the artifact bucket in S3. Seeing the artifact tells me that the CodeBuild was a success. 
The build process compiled the code from the source repository (GitHub), compressed it into a file, and stored that file into the S3 bucket.

![Image](http://learn.nextwork.org/genuine_navy_mysterious_monkey/uploads/aws-devops-codebuild-updated_d9cc6191)

---

## Automating Testing

<nil>

---

---
