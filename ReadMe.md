# Raspbian Kiosk Maker

Some ansible-powered steps to getting a super-simple information radiator on
a raspberry pi.

It works really well when used with [Dashing](http://shopify.github.io/dashing/)

This is designed for use in a LAN, so security is a bit rubbish.

# Steps

## Install rasbpian on the pi

I've done this via NOOBS, but a normal image should work.

Leave the system on the default "boot to CLI" setting.

* TODO: have a setup script to do this to an SD card via dd.
* TODO: include mdns / some sort of phone-home script by default
* TODO: allow overriding hostname at provision-time

## Install ansible locally

On the co-ordinating machine, ensure ansible is installed:

    [sudo] pip install ansible

## Define your inventory

See http://www.ansibleworks.com/docs/intro_inventory.html

Ensure all your desired kiosks are in a group called `kiosks`, and that each
host has a "page" variable.

    [pi]
    raspberry.local page=http://google.com

    [pi:vars]
    ansible_ssh_user=pi

    [kiosks:children]
    pi

I recommend saving this in a file local to this setup called `inventory`, rather
than using the global ansible inventory file.

## Make sure ansible can control the raspberry pis

Add your SSH key to the `pi` user, or use the `--ask-pass` option when running
ansible.

## Run ansible on the kiosks

    ansible-playbook -i inventory kiosk.yml