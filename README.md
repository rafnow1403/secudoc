# secudoc
Secure doc storage
# Preface
This repo is a proof of concept for entire SDLC. Main focus is on workflows, what should they do, when, and why. It should test whether shown approach will enhance security, speed and quality of deployed applications.
# 
With that in mind entire pipeline in steps:

1. On every push in every branch
 1. Unit test
 2. Build artifact and push it to image registry
 3. Scan for CVE (in this example AWS ECR scans it on push in step 2. Step 3 only checks status whether there's any CRITICAL vulnerability) (this might be consolidated with step 2)

2. On created release
  1. Unit test
  2. Build artifact and push it to image registry with SemVer tag
  3. Scan for CVE
  4. Update helm values.yaml in DEV
  5. Integration test
  6. Update helm values.yaml in STG
  7. Integration test
  8. Update helm values.yaml in PRD
  9. Integration test