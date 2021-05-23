# Git Flow with Azure DevOps CI/CD pipeline

**Author:** jason hsieh

**Date:** 2021/05/23

Main purpose is to share the experiences with using Microsoft Azure DevOps in the client-side and the design of Azure DevOps pipeline.

### **Simple introduction**

- The client where I'm involved in required using Azure DevOps in two projects

Two projects have different purpose and target environment:

- **Project A** is aimed to deploy the application to Windows Server 2016
- **Project B** where I involved more is aimed to deploy the application to Red Hat OpenShift

### Infrastructure as code

- One of the concept of DevOps is IaC

[https://docs.microsoft.com/zh-tw/dotnet/architecture/cloud-native/infrastructure-as-code](https://docs.microsoft.com/zh-tw/dotnet/architecture/cloud-native/infrastructure-as-code)

- With IaC, you automate platform provisioning. You essentially apply software engineering practices such as testing and versioning to your DevOps practices
- You can use some scripts such as YAML or even in Azure DevOps you can use Classic Editor and then turn the sequence to YAML, JSON, etc.

### Demo

- This demo section is for **Project A**
- **Project A** system information**:**
    - Java 8 `<java.version>1.8</java.version>`
    - Spring Boot
    - SQL Server 2016
    - Windows Server 2016
- The flow chart of Git Flow with Azure DevOps CI/CD pipeline

![](https://i.imgur.com/6Xmp56B.png)

- The below image describes that the flow from GitLab to build pipeline and release pipeline

![](https://i.imgur.com/gFmrN5O.png)

- ⚠️The continuous integration trigger can be set in build pipeline. However, if you have the condition of firewall issues, you may have to set the webHook or schedule the git pull/git push motion by yourself.
- The whole sequence of CI/CD:
    1. **Developer push code to GitHub and trigger build pipeline**

        It depends on which branch 👨🏽‍💻 push.

        For example, the build pipeline will be triggered if 👨🏽‍💻 push the code to his own branch and the code is merged into `develop`

        Hence, the build pipeline source code can be designed as `by repository` or `by branch`.

        Such as using `by branch` method and divided the environment into 4: DEV, SIT, UAT, PROD

        Branch names are going to be:

        - DEV
        - SIT
        - UAT
        - PROD

        So if 👨🏽‍💻 want to build SIT source code, he has to check the code in branch `SIT`

    2. **Build pipeline triggered and start to build source code**
        - Build pipeline will start executing the jobs and tasks

         ![](https://i.imgur.com/TFPnNYf.png)

        - You can view the process in the command mode:

        ![](https://i.imgur.com/osyVlhP.png) 

        - Every pipeline required an `agent` to run it, Microsoft divided it into **Self-hosted** and **[Microsoft-hosted](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser)**
            - Self-hosted ****agent run the pipeline with using your local resources
            - Microsoft-hosted agent run the pipeline with using the resources provided by Microsoft
            - In this demo I use Self-hosted ****agent in order to deploy the application on my [localhost](http://localhost) and client Windows Server
        - In this build pipeline, the task 1 of agent job will run below command:

            `mvn clean deploy -Dmaven.test.skip=true -PlocalDevOps=true`

        - task 2 will run `Copy Files to: $(build.artifactstagingdirectory)`
        - task 3 will run `Publish Artifact: buildArtifacts`, this is for publishing the build artifacts onto DevOps.
        - Finished the process of build pipeline
    3. **Build pipeline finished and start the release pipeline to deploy the artifacts to the target environment**
        - When the build pipeline finished it will trigger the release pipeline.

            ![](https://i.imgur.com/d2L6Euq.png)

        - In this release pipeline, the stage 1 of agent job will run `Download Artifacts`:
            - Release pipeline start to download the build Artifacts from the build pipeline
        - Stage 2 will start the command `java -jar` to deploy the application to target(localhost or Windows Server)
    4. **Deploy succeed and the CI/CD flow ended**
        - Checkout the deployment succeed or not via hostName:port
        - For example [localhost:8080](http://localhost:8080) or hostName:8080

    ### **It's a wrap**

    First time sharing the Azure DevOps experiences, feel free to pull out some questions or comment below. Please let me know how can I improve in the near future!

    Super sorry for my bad English lol, it has been quite a long time to write articles in English tho 😅

    More information for Azure DevOps please reference Microsoft official documents:

    [https://docs.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops](https://docs.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)

    Stay tuned for following updated 🤪