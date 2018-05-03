Example pipeline + code for Jenkins in OpenShift
================================================

This pipeline can take a couple of parameters when processing the pipeline template, check the ```parameters``` section in ```pipelines/pipeline-tempalte.yaml``` for more information.


``` bash
oc new-project demo-cicd
oc new-app jenkins-persistent
oc new-project demo-test
oc new-project demo-prod
oc policy add-role-to-user edit system:serviceaccount:demo-cicd:jenkins -n demo-test
oc policy add-role-to-user edit system:serviceaccount:demo-cid:jenkins -n demo-prod
oc process -f pipelines/pipeline-template.yaml | oc create -f - -n demo-cicd
```
