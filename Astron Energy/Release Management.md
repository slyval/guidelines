# Connect Release Strategy

## Purpose
This document provides guidance on how to manage the making of changes and their deployment from github to Commerce Cloud version 2 (CCv2) for the Astron Energy Connect ecommerce site. For uniformity and to avoid confusion between different people that might be involved in managing releases and deployments, it's important to adhere to the guidance provided. 

## Source Code Management
The commerce cloud platform allows for source code to be managed from any git based repository such as Github, Bitbucket, Gitlab among others. In the case of connect, Github is used. 

### Branching 
In order to manage the different stages that source code goes through from initial creation to deployment in production, multiple branches were created and are named below. 

 1. **master** - the master branch is the primary branch that is used to initiate any changes that need to be made. All users must checkout this branch to make any changes. If segregation of changes is required, feature branches can be created from the master branch and merged at a later point when the changes are ready to be integrated into the master branch. Creating feature branches for features or bug fixes is encouraged when multiple changes are being made concurrently. 
 2. **deployment** - the deployment branch is used as the development branch. This implies that all builds that need to be performed for the development environment will be done based on this branch. A pull request should be created from the master branch to the deployment branch once a release to development is required. All bug fixes for development should also be performed using a branch created of the deployment branch. 
 3. **quality_assurance** - this branch is used for all deployments to the staging environment. Builds for the staging environments should, therefore, be based on the quality_assurance branch. 
 4. **production** - the production branch is used for all releases to the production environment. Hotfixes for emergency issues in production should be performed off this branch

### Merging Control
There are control mechanisms that have been put in place to manage changes so that no unwanted changes are deployed without the necessary/requisite approvals

-	When merging requests from master to deployment, reviews are required and these will allow for changes to be reviewed and approved 
-	When merging pull requests from deployment to quality assurance, reviews and approvals are also required 
-	when merging requests from quality assut
	 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYwNzg4NTgzMywxNjM2NDg3NzMsLTI0MT
gyOTAwNywxMjE4ODY0MjYwLDExMTYwNDI5MDZdfQ==
-->