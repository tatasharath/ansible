# ansible

Ansible is a configuration management tool which works both on push and pull mechanisms

### INVENTORY File

Inventory is nothing but the list of machine ip or dns names that you want ansible to be managed.

Ansible by default installs 2.9 as by default on AMI's we will get python2. Always go with latest version, which can be installed from tools/ansible/install.sh

all is a group which included all the entries in INVENTORY File.

## Ansible is all about MODULES ( Collections )

ANSIBLE has lot of pre-defined variables and we need to use them to supply userName and password it has to use to authenticate to the nodes.

ans
### ansible_user     : Predefined variable for userName 
### ansible_password : Predefined variable for password  

### Variables should be passed to ansible by using the flag -e

    Ex: ansible -i INVENTORY all  -e ansible_user=userName -e ansible_password=password 
    $ ansible -i inv all  -e ansible_user=centos -e ansible_password=xyz123 -m shell -a uptime

    Automated Approach : This uses Playbook

Playbooks are written using a language called YAML.

    YAML is just  markup languaga ; Markup language is nothing a presentation language

    YAML is indendation specific.

### What is a playbook ?
    * Playbook : A Playbook is a list of plays ( and that's why it always starts with - )
    * Play     : A Play is a list of tasks.
    * Task     : A Task is nothing but an action that we wish to perform

### What is a fact ?
    In ansible, fact is the property of the node mentioned in the inventory file. By default, ansible is going to gather all the facts of the amchines mentioned in the inventory file

    $ ansible -i inv all -e ansible_user=centos -e ansible_password=abc@123 -m setup

### V.IMP Points

    NOTE : If you would just like to print a variable, then enclose the variable in "{{varName}}" and there is no single quote concept 4
    and if the variable is present in between the string of words, you don't have to enclose them in quotes.

### 1) No two tasks of a play can have the same name.

# Ansible Roles :
roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies

### When to use ansible pull vs ansible push ?

    1) When you infrastructure is static, then we will host an ANSIBLE Server and will target configuration management on all the nodes from your ansible server. 

    2) When you infrastrucre is not static, which means on cloud we often scale out and down the infra , in this case manintaining the inventory is quite challenging and to avoind this, what we do is, as a part of the BOOTSTRAPPING / INITIALIZATION , we will let the ansible-pull command to run.

### Points to be notes when using pull : 
    
    * Ensure your nodes running ansible has ansible installed.
    * Pull only works from GIT. 

### How to use ansible pull ?

    $ ansible-pull -U https://github.com/b55-clouddevops/ansible.git -e ENV=dev -e COMPONENT=mongodb roboshop-pull.yml

### Role Dependency :
    In Ansible terms, a dependency is any role that needs to have run before the current role runs.

### What is a handler and why do we need that ?
    Ansible provides feature named handlers, which is like a task but will only run when called by a notifier in another task. 

    This feature is important because your requirements for running a task may depend on the state of a service, existence of a file or a follow up tasks when state changed.

### Grok Debugger : Helps in converting unstructured logs to structured :
    grok    : https://grokdebugger.com/
    patterns: https://github.com/cjslack/grok-debugger/blob/master/public/patterns/grok-patterns

### Frontend Grok Pattern :

    %{HTTPDATE:log_timestamp}%{SPACE}%{IP:source_ip}%{SPACE}%{WORD:http_method}%{SPACE}%{PATH:http_path}%{SPACE}%{WORD}/%{NUMBER}%{SPACE}%{NUMBER:http_status:int}%{SPACE}%{NUMBER:bytes_sent}%{SPACE}%{NUMBER:response_time:float}
