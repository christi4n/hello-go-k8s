# Hello Go Automation with Ansible

There is a submodule "hello-go" in this repository. Do not forget to check from time to time any update with "git submodule update" in the root folder of this repository.
## Do some cleanup before running Ansible playbook

    kubectl delete namespace sample-go-apps
## Requirements for Ansible

You need to install the **kubernetes.core** Ansible Galaxy collection. You have two possibilities:

- You run with CLI the following command: ansible-galaxy collection install kubernetes.core
- Go to the root directory of this project and run the following command: ansible-galaxy collection install -r ansible/requirements.yml

## Credentials

You have to set first the correct information into ansible/vars/.registry-credentials.yaml
However, you should never have valid credentials in your git repository. You should therefore create a vault with ansible-vault.

    ansible-vault create --vault-password-file ansible/vault/registry-credentials.yml

This file contains the login and the password to your image registry, DockerHub for instance.
This file is called from inside the main.yml file because the two variables are required to connect to the image registry to publish the Docker image:

- docker_token
- docker_registry_username

You set first the password for the new Ansible vault then, edit the vault with your declared favorite editor.

    ---
    docker_token: some_token
    docker_registry_username: your_username

You need to have the password on your file system to decrypt the vault. Mine is in ansible/.vault_pass in the project but it is excluded from git versioning.

### View the credentials

    ansible-vault view ansible/vars/registry-credentials.yml --vault-password-file=ansible/.vault_pass

## Run Ansible playbook

    ansible-playbook -i $HOME/.ansible/path/to/inventory --vault-password-file=ansible/.vault_pass ansible/main.yml -vvv

You can also replace the --vault-password-file with --ask-vault-pass if you prefer to be asked the vault password.

You should use environment variables with CI/CD automation tools. GitLab CI gives you the advantage to set environment variables. Use $VAULT_PASSWORD with the correct password, then, add it in a basic script (vault_secret.sh):

    #!/bin/bash
    
    echo $VAULT_PASSWORD

Then, call ansible-playbook with "--vault-password-file vault_secret.sh".
In a local environment, you can set permanently the password in an environment variable:

For zsh:

    echo 'export VAULT_PASSWORD=myGreatPassw00rd' >> ~/.zshenv

For bash:

    echo 'export VAULT_PASSWORD=myGreatPassw00rd' >> ~/.bash_profile