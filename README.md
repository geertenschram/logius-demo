Example pipeline + code for Jenkins in OpenShift
================================================

``` bash
oc new-project demo-cicd
oc new-app jenkins-persistent
oc new-project demo-test
oc new-project demo-prod
oc policy add-role-to-user edit system:serviceaccount:demo-cicd:jenkins -n demo-test
oc policy add-role-to-user edit system:serviceaccount:demo-cid:jenkins -n demo-prod
oc create -f pipelines/development-pipeline.yaml -n demo-cicd
```
