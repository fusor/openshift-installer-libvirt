# openshift-installer-libvirt
This repo contains an ansible role to simplify running openshift-install with libvirt enabled.

## Instructions
1. export your pull secret using the env var `PULL_SECRET`
1. export the SSH public key using the env var `OCP_LIBVIRT_SSH_KEY`
1. export your `GOPATH` if you haven't already. It will be necessary in order to build openshift installer
1. edit `config.yml` to make desired changes to
  * `version` - The OpenShift Major.Minor version to deploy.
  * `base_domain` - Combined with the `cluster_name` to create the cluster FQDN.
  * `cluster_name` - Combined with the `base_domain` to create the cluster FQDN.
  * `cluster_bridge` - The name of the network bridge. The role defaults `cluster_name`, but can be different.
  * `cluster_network` - A class C network address. Should end with .0, for example 192.168.126.0.
  * `image_sha` - The SHA of a Major.Minor.Patch version. See the next section for how to get a SHA for an unlisted version.
  * `master_ram` - The amount of RAM for each master VM. My experience is 8 is doable, but 16 provisions more reliably.
  * `master_vcpu`- Number of CPU's per master VM.
  * `worker_ram` - TThe amoumt of RAM for each worker VM. My experience is 8 is doable, but 16 provisions more reliably.
  * `worker_vcpu` - Number of CPU's per master VM.
  * `workers`- The number of workers to deploy.
1. run `ansible-playbook create-cluster.yml`

To destroy the cluster when done run `ansible-playbook destroy-cluster.yml`.

## Finding additional versions
To find an image SHA for a version not listed check for the release.txt for that version at https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/

## Issues
There is no load balancer when using libvirt. Two ingress routers are created and can land on any worker. In order to ensure one of the routers will land where it is expected it is suggested that you do not make masters schedulable and only provision two workers.

If after bringing up the cluster you would like to deploy additional workers you can scale up by running a command like:  
`oc scale -n openshift-machine-api machineset --replicas=3 --all`
