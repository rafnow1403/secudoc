# secudoc
Secure doc storage
# Preface
This repo is a proof of concept for entire SDLC. Main focus is on workflows, what should they do, when, and why. It should test whether shown approach will enhance security, speed and quality of deployed applications.
# More Explanation
With that in mind entire pipeline in steps:

- On every push in every branch
 1. Unit test
 2. Build artifact and push it to image registry
 3. Scan for CVE (in this example AWS ECR scans it on push in step 2. Step 3 only checks status whether there's any CRITICAL vulnerability) (this might be consolidated with step 2)

- On created release
  1. Unit test
  2. Build artifact and push it to image registry with SemVer tag
  3. Scan for CVE
  4. Update helm values.yaml in DEV
  5. Integration test
  6. Update helm values.yaml in STG
  7. Integration test
  8. Update helm values.yaml in PRD
  9. Integration test

  # Dependencies
  Currently to make it work we need also a registry- ECR in this case
  Standalone deployment of ECR as well as OIDC provider with role and policies are in https://github.com/rafnow1403/infra-ecr
  
  To deploy this application I used EKS which is available in https://github.com/rafnow1403/14a-deploy-eks in terraform directory
  Rest of the repository was moved to separate infrastrucutre code from application's infra code where everyone can commit. These repos are:
  - https://github.com/rafnow1403/argocd-aoa-dev/tree/main repo for app of apps pattern. There are defined ArgoCD's Application resources which then point to the actual Helm/Kustomize/KCL in the app's repository (Helm chart is not done yet)
  - https://github.com/rafnow1403/argocd-commons-dev/tree/main repo for argocd itself, it's components as well as PROBABLY common resources like certmanager, stakater/reloader etc.
