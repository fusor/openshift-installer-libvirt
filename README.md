# openshift-installer-libvirt
This repo contains an ansible role to simplify running openshift-install with libvirt enabled.

## Instructions
1. export your pull secret using the env var PULL_SECRET
1. export the SSH public key using the env var OCP_LIBVIRT_SSH_KEY
1. export your GOPATH if you haven't already. It will be necessary in order to build openshift installer
1. edit default.yaml to make desired changes to
  * version
  * image_sha 
  * master_ram
  * master_vcpu
  * worker_ram
  * worker_vcpu
  * workers
1. run `ansible-playbook create-cluster.yml`

To destroy the cluster when done run `ansible-playbook destroy-cluster.yml`.

## Finding additional versions
To find an image SHA for a version not listed check for the release.txt for that version at https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/

