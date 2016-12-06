# Shared user management example with ansible

This is example for managing all of company admin users with ansible for all servers.

It has been tested and used in production with `Ubuntu` virtual servers. Other distros won't likely work at this time.

## What's included?
* Manage all your users with git and ansible
* Disables servers from asking for sudo password
* Disables ssh root access and password login for ssh

## How to setup
`group_vars/all/users.yml` file contains list of company super admins which should gain access to all created servers. Remove our example data and put list of your admins over there instead.

Create new folder inside `environments` for certain servers. For example these can be: `production`, `testing` or `clientnamehere-cluster`.

Then create inventory file for the servers. This is an example:
```
[servers]
xxx.xxx.xxx.xxx
yyy.yyy.yyy.yyy
```
You can use hostnames or ip-addresses here.

Then add people involved with certain environments into:
```
environments/{{your-environment}}/group_vars/all/users.yml
```

## Example data

This is an example how the final results look for [the author](https://github.com/onnimonni).

```yml
company_admin_list:
  - name: "Onni Hakala"
    username: "onnimonni"
    keys:
      active:
        - "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC8gOdvYe12yaVQGMZXgn0qcd7vxMZhj1wsBTDFP/HnwdnbywbV/jt//yGex51cTkfqoP3YlkdgUfQeKJws4j7rxu04gx6uRBIlRcR+7wdcFgksFs4EFumTpZOSOwKybHVIbeIubLi76+mhB9L2JXD4f+TtkL0WJtvBDsCavGLHbru2JafS3K/6b97sU2vpmA/mDREKDpnuVxcMXsXlY0THgoxx70L7SjYsWMzRYiJc+FWzMqyi1yribhEUuUT4ch6B7DiBuPfWFQUlA2KkGbihsw+Kmyrw0e36z1MOEWAhroczt8zKjawWeYQ4qTwQRrjM8b+C2yfFgBpUqFAhAM0Lb9dh8V3xfui30zik2eW2LTQ4JtD2xNUflA+NvG2+fcB7w3ub+QNXI3zp+Joto627oB3j4Nao+s/XHOd0T8idHVrbfhoRK8UE4r6nKI8b5b7JrzaDipE1CjS2TsIixaj9a/VxYMtNE2A9JPGHsXlIPpi++GaW+rz4DZoqkArfsFE= onni@keksi.io"
      disabled:
        - "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCsPoDM21LjBodcS3Kiq7ZDyNjFY4L03Yx2nSoSzdV7TKj00Zk8gh1hSSgI/xSDXiK+QEbfPuh/7+G3sfeDDNavjWUA1kdLve4D6MTI302C3HilK6wOZV67ezyAgdgf/VCHfx+no+bocw/05r1VamLrnFjDasWAQFFeFLaOnfQTArHQRl2Z8+4ZtZ1YQPhnleFQfEz546D3oWE1DS3vuXL1Qodz3gijtHhGnc3jaQM+7m/gg2vrcL6bc/vCtdunpA1fSQrBSx0kK4qdTWwc2LZDwKIT3YP5GthvgkFHsbCo2mNdBwWZfOBTYr5LBpz9YBcO7oVBWVEI00BdEPRFOYJL onnimonni@Onni-MacBook-Air.local"
    shell: "/bin/bash"
    state: present
  - name: "Example Person"
    username: "example-quitted-person"
    state: absent
```

I use the same username `onnimonni` locally and in servers so that I won't need to provide the username everytime to different programs.

I have new harder 4096bit ssh key and I want to make sure that my older 2048bit key is not present anywhere.

## How to deploy

This is how you would deploy all users for environment named `production` initially with user `root`. Please provide your ssh password for ansible.

```console
ansible-playbook -i environments/production users.yml -u root --ask-pass
```

**Notice: You only need to do this once! On the next run you can run it like this:**
```console
ansible-playbook -i environments/production users.yml -u your-user-name-here
```

## Maintainer
[Onni Hakala](https://github.com/onnimonni)

## License
MIT
