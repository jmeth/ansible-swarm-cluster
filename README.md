# Ansible Playbook :: Docker Swarm Cluster
---

This is a playbook for configuring a Docker Swarm cluster with a Consul back end across 1 or more hosts.  This is useful for testing more advanced Docker features such as networking and service discovery.

Current Features:
* Docker v1.9 Server and Client
* Consul 0.6.4
* Swarm Agent and Master

## Requirements

* Ansible 2.0
* Vagrant + `ansible-hosts` plugin (if running the dev playbook)
* CentOS 7.2 (if running your own infrastructure)

## Usage

Vagrant can be used to test the playbook and is currently configured for a 3 host cluster.  Remove references to swarm2 and swarm3 in the development inventory file and Vangrantfile if a single host is required.  

If running the playbook outside of Vagrant make sure the hosts are installed with CentOS 7.2 and DNS is configured.

Playbook currently requires a minimum of 1 host in both the `[leader]` and `[cluster]` groups.  This host acts as the Consul Boostrap node as well as the future Docker Registry server.  All extra hosts can be added to just the `[cluster]` group.

## Disclaimer

This playbook is primarily used for development and should not be considered production ready.  Testing has only been done so far on Vagrant boxes running CentOS 7.2 and most security settings are disabled at this time.  
