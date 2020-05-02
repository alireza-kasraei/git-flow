# git-flow branching model

there are two branches, used to record project history : **master** and **develop**.  

*   **master** is a branch, where the source code of **HEAD** always reflects a *production-ready* state.
*   **develop** serves as an integration branch for features or latest delivered development changes for the next release.

 as a brief overview of the process :

When the source code in the develop branch reaches a stable point and is ready to be released, all of the changes should be merged back into master **somehow** and then **tagged** with a release number.  


##### Supporting branches 
  
  there are three types of branches that serves as supporting branches to aid parallel development between team members,ease tracking of features, prepare for production releases and to assist in quickly fixing live production problems.  
they have a limited life time most of the times and they will be removed eventually.  
  

*   feature branches
*   release branches
*	hotfix branches
  

##### Feature Branches

usually branch off from **develop**, must merge back to **develop**, and we name it by the name of the feature that is developed by developers.  

example:  

*   switch to myfeature branch : `git checkout -b myfeature develop`
*   add feature
*	switch back to develop branch : `git checkout develop`
*	merge myfeature with develop : `git merge --no-ff myfeature`
*	delete myfeature branch : `git branch -d myfeature`
*	push it to origin : `git push origin develop`  

##### Release Branches

usually branch off from **develop**, must merge back to **develop** and **master**, and we name it with **release-*** naming convention. release branches contain a pre-determined amount of features and  should be deployed to a staging
server for QA testing. furthermore they allow for minor bug fixes and preparing meta-data for a release (version number, build dates , ...). by doing all of this work on a release branch, the **develop** branch is cleared to receive
features for the next big release. The key moment to branch off a new release branch from develop is when develop (almost) reflects the desired state of the new release. At least all features that are targeted for the release-to-be-built must be merged in to develop at this point in time. All features targeted at future releases may not—they must wait until after the release branch is branched off.

It is exactly at the start of a release branch that the upcoming release gets assigned a version number—not any earlier. Up until that moment, the develop branch reflected changes for the **next release**, but it is unclear whether that **next release** will eventually become 0.3 or 1.0, until the release branch is started. That decision is made on the start of the release branch and is carried out by the project’s rules on version number bumping.

example:

*	switch to release branch : `git checkout -b release-1.2 develop`
*	modify files
*	change the release version and commit : `git commit -a -m "Bumped version number to 1.2"`
*	merge it with master branch : `git checkout master` , `git merge --no-ff release-1.2`
*	tag the release : `git tag -a 1.2`
*	merge it with develop branch : `git checkout develop` , `git merge --no-ff release-1.2`
*	delete it : `git branch -d release-1.2`

##### Hotfix Branches

are defined as minor fixes to the each project release.  

branch off from **master**, must merge back to **develop** and **master**, and we name it with hotfix-* convention.

*	branch from master : `git checkout -b hotfix-1.2.1 master`
*	modify files
*	change the version number and commit : `git commit -a -m "Bumped version number to 1.2.1"`
*	try to separate fix commits from version change commit
*	switch back to master : `git checkout master`
*	merge it : `git merge --no-ff hotfix-1.2.1`
*	finally tag it : `git tag -a 1.2.1`
*	now merge it with develop : `git checkout develop`
* merge it : `git merge --no-ff hotfix-1.2.1`


very well documented and described by [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model)
