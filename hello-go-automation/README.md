# Hello Go Automation with Ansible

There is a submodule "hello-go" in this repository. Do not forget to check from time to time any update with "git submodule update" in the root folder of this repository.
## Do some cleanup before running Ansible playbook

    kubectl delete namespace sample-go-apps
## Requirements for Ansible

You need to install the **kubernetes.core** Ansible Galaxy collection. You have two possibilities:

- You run with CLI the following command: ansible-galaxy collection install kubernetes.core
- Go to the root directory of this project and run the following command: ansible-galaxy collection install -r ansible/requirements.yml

## Run Ansible playbook

    ansible-playbook -i $HOME/.ansible/path/to/inventory hello-go-automation/main.yml -vvv