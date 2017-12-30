# NetDevOps - Xenial Ansible Network Automation Framework
## Using Ansible for Common Networking Tasks
### Created:  2016-06-05  
### Modified: 2017-12-30
#### Claudia de Luna (claudia.deluna@dimensiondata.com)

Ubuntu based Container providing an Ansible 2.4 Control server and Python 2.7 environment along with an Ansible "getting started pack" for the Automation Framework Container.  The container provides a suitable environment for running scripts and playbooks from the [Emerging Technologies Team](https://dimensiondata.sharepoint.com/teams/ncgg1/SitePages/Main.aspx).

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

The automation framework solution needed to be portable and flexible enough to deal with the differences encountered across projects.  A solution that could exist on any laptop, desktop, a local VM etc.. is key.

Solution:

Docker Image - Provides a portable and reproducible environment that can be run on a laptop or desktop and which can leverage either the existing network or a client VPN connection
Ansible - Provides the framework to automate most of the target functions.  Its affinity with Python is a plus as we gain some nice integration with existing scripts (python and jinja2 in particular)


Progress:

Purpose built Docker image based on Ubuntu 16.04 (Xenial Xerus) with python, ansible, and a few other tools to make it a bit easier to work in the environment.
https://github.com/cldeluna/ansible2_4_base

This image is currently working on Windows 7 and Mac OS-X (next test will be Windows 10 but still using Docker Tookbox).

Basic set of Ansible playbook to 
- back up configurations 
- run a variety of show commands and save the output on Cisco IOS devices
- gather facts from Cisco IOS devices using the Ansible module
- gather facts from Cisco NX-OS devices using the Ansible module
- Generate semi-complex configurations
- Apply configurations

using standalone playbooks and roles.

Assumptions:
The environment will be on a laptop or destop running Windows 7 or Mac OS-X 10.11 (El Capitan) or later.
You have access to test network equiment either real or virtual.

Requirements:

 **Docker for OS-X or Docker Toolbox for Windows Installed and Working**

 Docker for OS-X (and Windows 10 but beware of the requirements and incompatabilities)
 https://docs.docker.com/engine/installation/

 Docker for Windows (Docker Toolbox)
 https://docs.docker.com/toolbox/toolbox_install_windows/

 **Some familiarity with Docker** (How to run an image and how to manage and interact with Docker containers)

 **Some familiarity with Ansible** (How to create, update, and run a playbook)
 Cisco IOS based network devices to run plays against
 

Steps:

1. Pull the Trusty Ansible Docker image from Dockerhub at https://hub.docker.com/r/cldeluna/xenial-ansible/

Pull the image from DockerHub.
```
$docker pull cldeluna/xenial-ansible
```
Confirm the image is available locally on your system.
```
$docker images
```
2. Instantiate an interactive container using that image (you will be in the /ansible directory)

Run a contaider from the image in interactive mode.
```
$docker run -it cldeluna/xenial-ansible
```
Your terminal will now be in the container and you can interact with the platform. For example, confirm the version of Ansible with this command:
```
$ansible --version
```
3. Pull the working files using git pull (git clone https://github.com/cldeluna/ansible2_4_base.git)

Confirm your location in the directgory.
```
$pwd
```
You should be in /ansible.

Clone the Base Ansible 2.4 repository which has a base Ansible environment.
```
$git clone https://github.com/cldeluna/ansible2_4_base.git
cd ansible2_4_base
```
4. Move to the ansible2_4_base directory
5. Update the hosts file to reflect the devices to act on. Sample entries are included but commented out. Update existing entries or create your own groups and inventory group variables.
6. Update the test-ios.yml or get_ios_commands.yml file to update the hosts and commands to execute.
7. With the get_ios_commands.yml playbook confirm that config backups have been saved in the /ansible/SHOW/ directory (get_ios_commands.yml)

Look for *.show commands.
```
$ls -al *.show
```
8. With the test-ios.yml playbook confirm that show command output has been saved in the /ansible/SHOW directory and that backups of the device configurations were saved to /ansible/backup.

Move into the /ansible/SHOW directory.
```
$cd SHOW
```
List files.
```
$ls -al
# Look for *.log files for the ouput of the show commands
```

Move into the /ansible/backup directory.
```
$cd backup
```
List files.
```
$ls -al
# Look for <device name>_config.<timestamp> files for configuration backups of each devices in the inventory group or device used in the playbook

✔ ~/ansible/ansible2_4_base/backup [master|…7] 
14:11 $ ls
arctic-sw03_config.2017-12-30@13:38:17
✔ ~/ansible/ansible2_4_base/backup [master|…7] 
14:11 $ 

```

The following sample playbooks are also included:

ios_facts.yml and nxos_facts.yml are playbooks which gather facts from ios and NX-OS devices respectively using the Ansible gather facts modules. 

ntp_role.yml
this utilizes the "ntp" role under the roles directory to generate a base ntp config snippet and serves as an example playbook for configurations using a role.

make_cfg.yml and apply.cfg make nexus base vpc pair configurations for a pair of nexus and then applies those configs.



## Things to work on next:

+ A Docker image based on Mint to provide a GUI
+ Add a few more tools and customizations to the Dockerfile
+ Add and Expand the Playbooks
+ Figure out how to get all the resulting log files and templates off the host
+ Format the show commands logs
+ Staging QA
+ More detailed instructions
+ Add Ansible Tower for Client Demos


<script src="https://gist.github.com/jonschlinkert/5854601.js"></script>