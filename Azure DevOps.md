# Azure DevOps Cheatsheet

# Azure DevOps Tutorial  
*Getting started with Azure DevOps for CI/CD, Project Management, and Code Collaboration.*

---

## 📌 Overview

Azure DevOps is Microsoft’s cloud-based platform for:
- **Source Control** (Git repositories or TFVC)
- **Work Tracking** (Boards, Kanban, Scrum)
- **Continuous Integration / Continuous Deployment (CI/CD)**
- **Artifact Management**
- **Test Management**

This tutorial will guide you through:
1. Setting up an Azure DevOps organization and project.
2. Managing code with Repos.
3. Planning work with Boards.
4. Creating automated builds with Pipelines.
5. Deploying with Releases.
6. Using Artifacts.

---

## 1️⃣ Create an Azure DevOps Organization and Project

### Steps:
1. **Sign in** to [Azure DevOps](https://dev.azure.com/).
2. Click **New organization** → give it a name.
3. Create a **New Project**:
   - Name: `MyFirstProject`
   - Visibility: Private (recommended for internal use)
   - Version Control: **Git**
   - Work Item Process: **Basic** (or Scrum/Kanban if you prefer)

📌 **Tip:** Each project is like a workspace that contains code, boards, pipelines, and artifacts.

---

## 2️⃣ Azure Repos – Managing Your Code

Azure Repos supports **Git** (distributed) and **TFVC** (centralized).  
We’ll use Git here.

### Clone Your Repo
```bash
# Clone repository to local machine
git clone https://dev.azure.com/{organization}/{project}/_git/{repo-name}

cd {repo-name}
```

### Push Code to Azure Repos
```bash
# Create a new file
echo "# My Azure DevOps Project" > README.md

# Commit and push
git add README.md
git commit -m "Initial commit"
git push origin main
```

---

## 3️⃣ Azure Boards – Managing Work

Azure Boards is great for tracking tasks, bugs, and features.

### Create a Work Item
1. Go to **Boards → Work items → New Work Item**.
2. Choose **Task**, **Bug**, or **User Story**.
3. Assign it to a user, set a priority, and save.

📌 **Tip:** Use the **Backlog** view for sprint planning, and **Board** view for Kanban-style tracking.

---

## 4️⃣ Azure Pipelines – CI/CD

Pipelines automate your build, test, and deployment process.

### Create a YAML Pipeline
1. Go to **Pipelines → Create Pipeline**.
2. Select **Azure Repos Git** → choose your repository.
3. Use the following example `azure-pipelines.yml`:

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: UseNode@2
  inputs:
    version: '18.x'

- script: |
    npm install
    npm test
  displayName: 'Install dependencies and run tests'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
```

4. Commit and run the pipeline.

---

## 5️⃣ Azure Releases – Deploying Your App

1. Go to **Pipelines → Releases** → **New Release Pipeline**.
2. Add an **Artifact** from your build pipeline.
3. Add a **Stage** (e.g., "Dev", "QA", "Prod").
4. Configure **Deploy tasks** (Azure App Service, Kubernetes, VM, etc.).
5. Trigger automatically after build completion or manually.

---

## 6️⃣ Azure Artifacts – Package Management

Azure Artifacts lets you host **NuGet**, **npm**, **Maven**, and **Python** packages.

### Example: Publish npm package
```bash
npm install -g azure-devops-node-api
npm login --registry=https://pkgs.dev.azure.com/{organization}/_packaging/{feed-name}/npm/registry/ --scope=@my-scope
npm publish
```

---

## 🔧 Best Practices

- **Branch Strategy:** Use GitFlow or trunk-based development.
- **Code Reviews:** Use Pull Requests with mandatory reviewers.
- **Secrets Management:** Store secrets in **Azure Key Vault** and link them in pipelines.
- **Build Validation:** Require PR builds to pass before merge.
- **Pipeline Templates:** Reuse YAML templates for consistent CI/CD.

---

## 📚 Useful Links
- [Azure DevOps Documentation](https://learn.microsoft.com/azure/devops/)
- [YAML Pipeline Reference](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema)
- [Azure Repos Git Guide](https://learn.microsoft.com/azure/devops/repos/git/)

---

✅ **You now have a full DevOps workflow** — from code commit → CI → deployment → work tracking.  
Next step: start automating more processes and integrating with other Azure services.

# 🚀 Advanced Azure DevOps Guide  
*Real-world CI/CD with multi-stage pipelines, approvals, and deployment strategies.*

---

## 📌 Overview  

This guide builds on the [Beginner Azure DevOps Tutorial](#) and covers **production-grade** Azure DevOps usage:  

- Multi-stage YAML pipelines (Build → Test → Deploy)  
- Environment-based deployments (Dev, QA, Staging, Prod)  
- Approval gates & manual interventions  
- Blue/Green & Canary deployment strategies  
- Integration with **Azure Key Vault** for secrets  
- Pipeline caching for faster builds  
- Artifact versioning and tagging  

---

## 1️⃣ Advanced Git & Branching Strategy  

A strong branching strategy ensures stability and fast delivery.  

**Recommended: GitFlow or Trunk-Based Development**  

```plaintext
main     → Production-ready code  
develop  → Active development branch  
feature/* → Short-lived branches for new features  
release/* → Pre-production stabilization  
hotfix/*  → Critical fixes for production  
```

**Azure DevOps Policies**:
- Require PR build validation before merge  
- Require at least **2 reviewers**  
- Enforce branch naming conventions with **branch policies**  
- Automatically delete source branches after merge  

---

## 2️⃣ Multi-Stage YAML Pipeline Example  

Below is a **full production CI/CD** pipeline for a Node.js app.  

```yaml
trigger:
  branches:
    include:
      - main
      - develop

variables:
  node_version: '18.x'
  artifact_name: 'webapp'
  build_configuration: 'Release'

stages:
- stage: Build
  displayName: 'Build Stage'
  jobs:
    - job: BuildJob
      pool:
        vmImage: 'ubuntu-latest'
      steps:
      - task: UseNode@2
        inputs:
          version: $(node_version)
      - script: |
          npm ci
          npm run build
        displayName: 'Install & Build'
      - task: PublishBuildArtifacts@1
        inputs:
          PathtoPublish: 'dist'
          ArtifactName: $(artifact_name)

- stage: Test
  dependsOn: Build
  jobs:
    - job: TestJob
      pool:
        vmImage: 'ubuntu-latest'
      steps:
      - task: UseNode@2
        inputs:
          version: $(node_version)
      - script: |
          npm ci
          npm test
        displayName: 'Run Unit Tests'

- stage: Deploy_Dev
  displayName: 'Deploy to Dev'
  dependsOn: Test
  jobs:
    - deployment: DevDeploy
      environment: 'Dev'
      strategy:
        runOnce:
          deploy:
            steps:
            - script: echo "Deploying to Dev..."
            - task: AzureWebApp@1
              inputs:
                azureSubscription: 'MyAzureServiceConnection'
                appName: 'my-dev-app'
                package: $(Pipeline.Workspace)/$(artifact_name)

- stage: Deploy_Prod
  displayName: 'Deploy to Production'
  dependsOn: Deploy_Dev
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
    - deployment: ProdDeploy
      environment: 'Prod'
      strategy:
        runOnce:
          deploy:
            steps:
            - script: echo "Deploying to Production..."
            - task: AzureWebApp@1
              inputs:
                azureSubscription: 'MyAzureServiceConnection'
                appName: 'my-prod-app'
                package: $(Pipeline.Workspace)/$(artifact_name)
```

---

## 3️⃣ Approvals & Gates  

**Why?** Ensure human review before critical deployments.  

**How to Set Up:**
1. Go to **Pipelines → Environments**  
2. Select your environment (e.g., "Prod")  
3. Enable **Approvals and Checks**:
   - Add required approvers  
   - Add a **Service Health Check** to block deployment if Azure services are down  
   - Add a **Query Work Items Check** to ensure all linked work items are resolved  

---

## 4️⃣ Deployment Strategies  

### **Blue/Green Deployment**
- Two identical environments: Blue (current) & Green (new)  
- Switch traffic to Green only after successful validation  

```yaml
strategy:
  runOnce:
    preDeploy:
      steps:
        - script: echo "Running smoke tests on Green..."
    deploy:
      steps:
        - script: echo "Deploying to Green..."
    routeTraffic:
      steps:
        - script: echo "Switching traffic to Green..."
```

### **Canary Release**
- Gradually route traffic to the new version (10% → 50% → 100%)  

---

## 5️⃣ Secrets with Azure Key Vault  

1. Create **Azure Key Vault** in the portal.  
2. Add secrets (e.g., `DB_PASSWORD`).  
3. In Azure DevOps Pipeline YAML:  

```yaml
- task: AzureKeyVault@2
  inputs:
    azureSubscription: 'MyAzureServiceConnection'
    KeyVaultName: 'my-keyvault'
    SecretsFilter: '*'
```

📌 Secrets will be available as `$(DB_PASSWORD)` in the pipeline.  

---

## 6️⃣ Pipeline Caching for Faster Builds  

```yaml
- task: Cache@2
  inputs:
    key: 'npm | "$(Agent.OS)" | package-lock.json'
    restoreKeys: |
      npm | "$(Agent.OS)"
    path: $(Pipeline.Workspace)/.npm
```

---

## 7️⃣ Artifact Versioning & Tagging  

Tag builds automatically:  

```yaml
- script: |
    git tag v$(Build.BuildNumber)
    git push origin v$(Build.BuildNumber)
  displayName: 'Tag Release'
```

---

## 📚 Additional Recommendations  

- **SonarCloud Integration** → Code quality and security scanning  
- **Infrastructure as Code** → Use Terraform or Bicep in pipelines  
- **Test Automation** → Integrate Postman/Newman or Selenium tests  
- **Monitoring Hooks** → Notify teams via Teams/Slack on failures  

---

## 🔗 Useful Links  
- [Multi-Stage Pipeline Docs](https://learn.microsoft.com/azure/devops/pipelines/process/stages)  
- [Azure Deployment Strategies](https://learn.microsoft.com/azure/devops/pipelines/deploy/strategies)  
- [Key Vault Task](https://learn.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault)  

---

✅ You now have a **full production-ready DevOps pipeline** with approvals, environments, secrets, and deployment strategies.
