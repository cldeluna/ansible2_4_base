# NetDevOps - Trusty Ansible Network Automation Framework
## Using Ansible for Common Networking Tasks
### Created:  2016-06-05  
### Modified: 2027-10-15
#### Claudia de Luna (claudia@indigowire.net)


The "Trusty Ansible project" grew from the desire to apply many of the DevOps principles to my day to day networking work and my desire to learn Ansible.

These principles include:

- Automation
- Repeatability
- Self Documenting
- Idempotent
- Immutable platform
- Portability
- Flexibility

My goal was to develop a platform that would let me achieve all those things using relatively easy, cross platform, flexible tools and methods.

To put this in proper context, I am a professional services network delivery engineer focused on data centers.  The most common network operating systems I work with are Cisco IOS (and other IOS flavors) and NX-OS based systems.

Target Functions:
- Automate the development of device specific configurations from templates and source data
- Automate the backup of configurations
- Automate the running of "snapshot" and "discovery" show commands
- Automate configuration updates
- Automate testing and validation

The ability to instantiate a "framework" to achieve these tasks across Windows, Macintosh, and Linux based systems including laptops was a key requirement.

Common Tasks that need to be executed on most, if not all, projects.

* Run a variety of show commands for both initial discovery, baselines, and testing.
* Compare existing configurations against a master template both to assess impact of change and for compliance.
* Push out configuration updates
* Audit Configurations
* Testing

In many cases I'm either on site or traveling with my laptop so the solution needed to be portable and flexible enough to deal with the differences I encounter across projects.  A solution that could exist on my laptop, my desktop, a local VM etc.. is key.

Solution:

Docker Image - Provides a portable and reproducible environment that can be run on a laptop or desktop and which can leverage either the existing network or a client VPN connection
Ansible - Provides the framework to automate most of the target functions.  Its affinity with Python is a plus as we gain some nice integration with existing scripts (python and jinja2 in particular)


Progress:

Purpose built Docker image based on Ubuntu 16.04 (Xenial Xerus) with python, ansible, and a few other tools to make it a bit easier to work in the environment.
https://github.com/cldeluna/trusty-ansible

This image is currently working on Windows 7 and Mac OS-X (next test will be Windows 10 but still using Docker Tookbox).

Simplistic Ansible playbook to back up configurations and run a variety of show commands and save the output on Cisco IOS devices.  I say simplistic because I want to move to a role based approach for eaven greater portability.

Assumptions:
The environment will be on a laptop or destop running Windows 7 or Mac OS-X 10.11 (El Capitan) or later.
You have access to test network equiment either real or virtual.

Requirements:
 Docker for OS-X or Toolbox Installed and Working
 Some familiarity with Docker (How to run an image and how to manage and interact with Docker containers)
 Some familiarity with Ansible (How to create, update, and run a playbook)
 Cisco IOS based network devices to run plays against
 

Steps:

1. Pull the Trusty Ansible Docker image from Dockerhub
2. Instantiate an interactive container using that image (you will be in the /ansible directory)
3. Pull the working files using git pull (git https://github.com/cldeluna/ansible2_4_base.git)
4. Update the hosts file to reflect the devices to act on
6. Update the test-ios.yml file to update the hosts and commands to execute.
7. Confirm that config backups have been saved in the /ansible/backup directory
9. Confirm that show command output has been saved in the /ansible/show_commands directory


Things to work on next:

+ A Docker image based on Mint to provide a GUI
+ Add a few more tools and customizations to the Dockerfile
+ Add and Expand the Playbooks
+ Figure out how to get all the resulting log files and templates off the host
+ Custom module for the auditing function?
+ Format the show commands logs
+ Staging QA
+ More detailed instructions
+ Move to roles

