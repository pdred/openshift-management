# Jobs

This directory contains a collection of [jobs](https://docs.openshift.com/container-platform/latest/dev_guide/jobs.html) and [scheduled jobs](https://docs.openshift.com/container-platform/latest/dev_guide/scheduled_jobs.html).

The rest of this document describes the specific configurations that are applicable to the execution of certain jobs contained within this directory.

### Pruning Resources

The [scheduledjob-prune-images.json](scheduledjob-prune-images.json) facilitates [image pruning](https://docs.openshift.com/container-platform/latest/admin_guide/pruning_resources.html#pruning-images) of the integrated docker registry while the [scheduledjob-prune-builds-deployments.json](scheduledjob-prune-builds-deployments.json) facilitates pruning [builds](https://docs.openshift.com/container-platform/latest/admin_guide/pruning_resources.html#pruning-builds) and [deployments](https://docs.openshift.com/container-platform/latest/admin_guide/pruning_resources.html#pruning-deployments) on a regular basis.

Prior to instantiating the template, the following must be completed within a project:

1. Create a new service account

	```
	oc create serviceaccount pruner
	```

2. Grant cluster *edit* permissions on the service account created previously (requires elevated rights)

	```
	oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:<project-name>:pruner
	```

Instantiate the template

```
oc process -v=JOB_SERVICE_ACCOUNT=pruner -f <template_file> | oc create -f-
```

*Note: Some templates require additional parameters to be specified. Be sure to review each specific template contents prior to instantiation*

