


### Examples of templatizing arguments

Some arguments in the CI integrations are populated dunalically when the CI is triggered. You can templatize the values of these arguments to derieve the correct results.
The examples below relate to three arguments you are most likely to templatize and 

### Codefresh Classic
Codefresh Classic provides a list of system variables. Take advantage of these variable to define the value struture of the arguments.
#### CF_IMAGE
1. Example:  
**Value syntax**
${{CF_REPO_OWNER}}/${{CF_REPO_NAME}}:${{CF_BRANCH_TAG_NORMALIZED}}-${{CF_SHORT_REVISION}}
where:
${{CF_REPO_OWNER}} will provide the owner of the repository
${{CF_REPO_NAME}} is the name of the repositoru
${{CF_BRANCH_TAG_NORMALIZED}} gets the normalized version of the branch name, without invalid characters in case the branch name was as the Docker image tag name. 
${{CF_SHORT_REVISION}} to tag the image with the abbreviated 7-character revision hash, as used in Git.

**Result in CI image**


2. Example: Use a specific image tag
This example illustrates how to define the value when you know the specific image version to use.

**Value syntax**
%raw ${{CF_REPO_OWNER}}/${{CF_REPO_NAME}}:<v1.0>`
where:
${{CF_REPO_OWNER}} and ${{CF_REPO_NAME}} are the names of the repository owner and the repository respectivley. 
<v1.0> is the specific tag to use for the image and is hard-coded.


3. Example: Tag image with the latest Git tag available on your repository

**Templatized value**
`codefresh/${{CF_REPO_NAME}}:latest`
where:
`codefresh` is the hard-coded re
${{CF_REPO_NAME}} is the name of the repository that triggered the integration. 

`latest` tags image with the latest Git tag available for the repository defined by ${{CF_REPO_NAME}}.
