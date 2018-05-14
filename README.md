Example pipeline + code for Jenkins in OpenShift
================================================

This is an example pipeline for OpenShift + Jenkins, using the declarative Jenkins syntax. It is meant to showcase some of the options you can use to produce readable, modularized pipelines.

Basic Example using mostly defaults
-----------------------------------

This creates three projects: `demo-cicd`, `demo-test`, and `demo-prod`. It adds
a Jenkins instance with persistent storage to the `demo-cicd` project, then
sets permissions on the other two projects to grant the Jenkins service account
edit rights.  
The final step processes the BuildConfig template to create the pipeline.

``` bash
oc new-project demo-cicd
oc new-app jenkins-persistent
oc new-project demo-test
oc new-project demo-prod
oc policy add-role-to-user edit system:serviceaccount:demo-cicd:jenkins -n demo-test
oc policy add-role-to-user edit system:serviceaccount:demo-cid:jenkins -n demo-prod
oc process -f pipelines/pipeline-template.yaml | oc create -f - -n demo-cicd
```

Available Parameters
--------------------
* `APPLICATION_NAME`  
  The name for your DeploymentConfig, BuildConfig, etc.
* `TEMPLATE`  
  The template file that defines the DeploymentConfig, ImageStream, BuildConfig, Service, and Route.
* `PROJECTBASE`  
  The basename of the projects to use. For example, `demo` in `demo-cicd`, `demo-test`, and `demo-prod`.
* `SOURCE_REPOSITORY_URL`  
  The URL of your Git Repository
* `SOURCE_REPOSITORY_REF`  
  The branch to use
* `CONTEXT_DIR`  
  The subdirectory that holds the actual code in the source repository

Example overriding some settings
--------------------------------

This example creates the same application as the previous one, but using different project namespaces and a different Git repository.


``` bash
oc new-project logius-cicd
oc new-app jenkins-persistent
oc new-project logius-test
oc new-project logius-prod
oc policy add-role-to-user edit system:serviceaccount:logius-cicd:jenkins -n logius-test
oc policy add-role-to-user edit system:serviceaccount:logius-cicd:jenkins -n logius-prod
oc process -f pipelines/pipeline-template.yaml -p PROJECTBASE=logius -p APPLICATION_NAME=flopsels -p SOURCE_REPOSITORY_URL=https://github.com/geertenschram/logius-demo.git | oc create -f - -n logius-cicd
```
