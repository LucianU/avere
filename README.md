# avere
Un site pentru colectarea și gestionarea declarațiilor de avere.


## Development
Development is done in a Vagrant-managed VM. This ensures compatibility with
production. To setup your environment:
- install VirtualBox
- install Vagrant
- install Ansible and passlib:

        pip install ansible passlib

- install the Ansible roles:

        ansible-galaxy install ansible/roles/external.yml

Now you can run:
- `vagrant up` to create the VM and install everything
- `vagrant ssh` to log into the VM
- `./dj_server` to start the Django development server

That's it. Access the site at `http://localhost:8000`.

As an additional step, after you create the hosted repo, set its URL as the
value of `site_repo_url` variable in `ansible/group_vars/all.yml`.

### Useful Vagrant commands
- `vagrant up`: create a VM or start an existing one
- `vagrant provision`: run the Ansible code to provision the VM
- `vagrant suspend`: save the state of the VM and stop it.

For other Vagrant commands, `vagrant help` in the terminal.

## Deployment
By default, there are two deployment targets: staging and production. Start
by adding your host to each target's inventory file. You find that in
`ansible/inventory`. If this is the first deployment, you prepare the remote
host by running:

    ansible-playbook ansible/secure.yml -kK -u <user>

`<user>` should have superuser rights on the target machine.

Before you run the next command, you should set a value for
`nginx_server_name` in `ansible/group_vars/staging.yml` and
`ansible/group_vars/production.yml` respectively.

Now you can deploy by running:

    ansible-playbook ansible/staging.yml -K

For production, there is a corresponding playbook called `production.yml`.
