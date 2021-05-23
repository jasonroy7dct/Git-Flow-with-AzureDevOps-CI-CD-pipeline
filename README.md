# Git Flow with Azure DevOps CI/CD pipeline

**Author: jason hsieh**

### **Simple introduction**

- The client where I'm involved in required using Azure DevOps in two projects

Two projects have different purpose and target environment:

- **Project A** is aimed to deploy the application to Windows Server 2016
- **Project B** where I involved more is aimed to deploy the application to Red Hat OpenShift

### Demo

- This demo section is for **Project A**
- The flow chart of Git Flow with Azure DevOps CI/CD pipeline

![](https://i.imgur.com/6Xmp56B.png)

- The below image describes that the flow from GitLab to build pipeline and release pipeline

![](https://i.imgur.com/gFmrN5O.png)

- The continuous integration trigger can be set in build pipeline. However, if you have the condition of firewall issues, you may have to set the webHook or schedule the git pull/git push motion by yourself.

### Infrastructure as code

- One of the concept of DevOps is IaC

[https://docs.microsoft.com/zh-tw/dotnet/architecture/cloud-native/infrastructure-as-code](https://docs.microsoft.com/zh-tw/dotnet/architecture/cloud-native/infrastructure-as-code)

- With IaC, you automate platform provisioning. You essentially apply software engineering practices such as testing and versioning to your DevOps practices
- You can use some scripts such as YAML or even in Azure DevOps you can use Classic Editor and then turn the sequence to YAML, JSON, etc.
