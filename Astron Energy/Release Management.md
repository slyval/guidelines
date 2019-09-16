# Connect Release Strategy

## Purpose
This document provides guidance on how to manage the making of changes and their deployment from github to Commerce Cloud version 2 (CCv2) for the Astron Energy Connect ecommerce site. For uniformity and to avoid confusion between different people that might be involved in managing releases and deployments, it's important to adhere to the guidance provided. 

## Source Code Management
The commerce cloud platform allows for source code to be managed from any git based repository such as Github, Bitbucket, Gitlab among others. In the case of connect, Github is used. 

### Branching 
In order to manage the different stages that source code goes through from initial creation to deployment in production, multiple branches were created and are named below. 

 1. **master** - the master branch is the primary branch that is used to initiate any changes that need to be made. All users must checkout this branch to make any changes. If segregation of changes is required, feature branches can be created from the master branch and merged at a later point when the changes are ready to be integrated into the master branch. Creating feature branches for features or bug fixes is encouraged when multiple change

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDkzMTA0MDE1XX0=
-->