# Packer - Vagrant Box

This project provides basic template for building a Vagrant box with nginx package installed

## Download Packer

Packer installer can be downloaded from this [page](https://www.packer.io/downloads.html)

## Install Packer

Here is [install instructions](https://www.packer.io/intro/getting-started/install.html#precompiled-binaries) for pre-compiled binary

## Build Vagrant box

Running build command:

`packer build template.json`

## What's in template.json?

The [template.json](template.json) is packer file with descriptive resources.
It supports various components as per [Template Structure](https://www.packer.io/docs/templates/index.html#template-structure) document.
In this sample, it declares `variables` to demonstrate configurable pieces of information and the `builders` to perform build task(s) to build a machine with specific `type`.

**Provisioners**
Provisioners will be used to perform additional tasks on the machine created by the builders such as install and configure software.
With this mini-sample, it uses `shell` provisioner with external script, `scripts/provision.sh`, to install `nginx` package.

```
  "provisioners": [
    {
      "execute_command": "echo 'vagrant' | {{.Vars}} sudo -E -S bash '{{.Path}}'",
      "script": "scripts/provision.sh",
      "type": "shell"
    }
  ],
```

**Post-processors**
Post-processors are steps to perform against the built images.

## Download Vagrant

Vagrant installer can be download from this [page](https://www.vagrantup.com/downloads.html)

## Install Vagrant

Here is the [install instructions](https://www.vagrantup.com/docs/installation/)

## Import Vagrant

The output from Packer build process is Vagrant box file which can be imported directly into Vagrant using command:

`vagrant box add --name nginx64 packer-nginx64-vbox.box`

## Running the box

Create an empty directory for Vagrant workspace and navigate into the directory then execute command:

`vagrant init -m nginx64`

The result is a **Vagrantfile** being created with very basic properties.
To start up the box using `vagrant up` command in the same directory and ensure the box is up and accessible by using `vagrant ssh`.

## Where is nginx?

The command `vagrant ssh` will let you into the box then a command of your choices can be executed to check service status, for example

`service nginx status`

## Destroy Vagrant box

Once done with the activity, the box can be cleaned up using command `vagrant destroy` in the same Vagrant workspace directory.

## Running Test

The file `.kitchen.yml` gives you a brief demonstration of VirtualBox driver integration in Kitchen framework.

Converge will perform environment preparation task where it runs up the machine.

`kitchen converge`

Verify will perform verification task using `test/integration/default/default_test.rb` using the following command:

`kitchen verify`

Clean up test environment by running `destroy` command:

`kitchen destroy`

Alternatively, running a single `test` command for full test cycle:

`kitchen test`