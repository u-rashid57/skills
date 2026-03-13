---
name: pipeline-inspector
description: Advanced structured troubleshooting, optimization, and guidance for CI/CD pipelines with YAML examples for Azure DevOps, GitHub Actions, and hybrid environments.
license: MIT
metadata:
  author: u-rashid57
  version: "1.0.0"
  domain: utilities
  output-format: markdown
---

# Pipeline Inspector Skill

## Purpose
This skill enables the agent to:  
- Design, debug, and optimize CI/CD pipelines for speed, reliability, and security  
- Fix deployment issues in Azure DevOps, GitHub Actions, and multi-cloud pipelines  
- Provide secure, maintainable, and scalable pipeline guidance  
- Generate, validate, and modularize YAML pipeline templates  
- Analyze historical runs to detect patterns, recurring failures, and optimization opportunities  

## Platforms Supported
- **Azure DevOps** (YAML & Classic pipelines)  
- **GitHub Actions**  
- **Azure Repos & GitHub Repositories**  
- **Azure App Services, Functions, AKS, Docker, Kubernetes**  
- **Infrastructure as Code:** ARM, Bicep, Terraform  
- **Multi-cloud & Hybrid Pipelines:** AWS CodePipeline, GCP Cloud Build  

---

## Enhanced Troubleshooting Framework

### 1. Identify Platform & Environment
- Detect pipeline type: Azure DevOps, GitHub Actions, or multi-cloud hybrid  
- Identify agent/runner version, OS, and pool configuration  
- Detect environment readiness and prerequisites  

### 2. Identify Pipeline Stage
- Build / Test / Package / Deploy / Post-Deploy / Rollback  

### 3. Check Common Issues & Metrics
- YAML syntax & structural errors  
- Artifact publishing/consumption & dependency graph impact  
- Authentication, permissions, and secret/token scope  
- Runner / agent configuration & scaling  
- Caching, dependency restore, and flakiness detection  
- Container build and deployment errors (Docker/Kubernetes/Helm)  
- Pipeline metrics: slow builds, timeout risks, flakiness scoring  
- Cross-pipeline impacts for shared libraries or templates  

### 4. Log Analysis & Root Cause Detection
- Identify first failure occurrence and recurring patterns  
- Detect dependency errors, version mismatches, timeouts  
- Correlate failed vs. successful runs  
- Secret/variable misuse or misconfiguration  
- Assign error severity and impact rating  

---

## 5. Provide Solutions & Recommendations

**Problem → Root Cause → Fix → Prevention → Optimization**

### Example: Azure DevOps YAML Artifact Issue
**Problem:** Pipeline fails to consume published artifact.  
**Root Cause:** Artifact is published to a different pipeline stage without `publish`/`download` linkage.  

**Immediate Fix YAML Example:**
```yaml
# Stage: Build
- task: PublishPipelineArtifact@1
  inputs:
    targetPath: '$(Build.ArtifactStagingDirectory)'
    artifact: 'drop'

# Stage: Deploy
- task: DownloadPipelineArtifact@2
  inputs:
    artifact: 'drop'
    path: '$(Pipeline.Workspace)'