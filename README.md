# Jenkins PHP Ansible

A set of playbooks for setting up a Jenkins server and LAMP hosts for a development servers.

This will not work out of the box, some setup is required.

Copy `var/main-vars.sample.yml` to `var/main-vars.yml` and updated the varaibles for your setup.

## Encrypt String

To encrypt passwords using `ansible-vault encrypt_string` you will need a password file. Check your ansible config for the expected path i.e. `vault_password_file = ~/.ansible/vault-password`. The password in this file will be stored in plain text, enusre it is in a safe place and you keep a copy of the values you have encrypted. I recommend [1Password](https://1password.com/)

## Variables

    remote_port: 7822

Set the remote ssh port to change the default 22

    # User Playbook
    user_git_ssh_add_keys:
        - /Users/macusername/.ssh/id_rsa.pub
        - https://github.com/user.keys

ssh Keys to include

    user_git_ssh_remove_keys: []

If you wish to remove ssh Keys

    user_git_full_name: "Michael Porter"
    user_git_email_address: "devops@example.com"
    user_git_editor: nano

Git template settings.

    # pip install passlib
    # python -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.using(rounds=5000).hash(getpass.getpass()))"
    # gitpass
    # ansible-vault encrypt_string 'STRING_FROM_OUTPUT' --name 'user_git_password'
    user_git_password: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            61376430326233356231396130326135323138376437653037393632333933333231326566323835
            6162346637326536666136303034316334616633623164360a653663353835393266366431626632
            33633839326631616435396164356139363938386437376530373465366561303237376362343134
            6161376666363364340a613765303864633136353663323633613265613930643133646366313035
            37643330326536386461666165653431393463323737343863663436653934396335336166383237
            62653964323939333339303339626564653738613337383666633933396165616263373162303066
            61303939613964313064656461366561306435393966653331383266633234646532316432353838
            62393235313835393138376264333639336536313264633133326661623634373162636164313931
            37313132376634326238663862613536383761356536336465353061363763393430

Sets the password for git user.

    user_sudouser_username: "demo"
    user_sudouser_full_name: "Michael Porter"
    user_sudouser_ssh_add_keys:
        - /Users/macusername/.ssh/id_rsa.pub
    user_sudouser_ssh_remove_keys: []
    # pip install passlib
    # python -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.using(rounds=5000).hash(getpass.getpass()))"
    # sudopass
    # ansible-vault encrypt_string 'STRING_FROM_OUTPUT' --name 'user_sudouser_password'
    user_sudouser_password: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            33303334313764363663333839313035393365313435303066663735633530336562313964663361
            3630396634653534393236336630626336623834646564610a646538376465313137303462386330
            33666666656438343563313130353066376330366135626565373033326437386538653431393264
            6131663466666165640a366430376165316362376538613162623838386639323565613961626532
            39313762343261646362313533386136636339636535346131656362323234323664633965633836
            32396265623037376465663030373237336136636631383433393238616636373339623032653031
            31333632343365373337363332623234663838643435323263386535623639373263326465383234
            38646135616265323366326134323737383365336331353939633837346465333030383032323461
            36353632393065386462383033323334333931353138316262333364393961333037

Settings for the sudo user.

    # Certbot
    certbot_email: "devops@example.com"
    certbot_create_if_missing: yes
    # Install from source to get dns plugin folders
    certbot_install_from_source: yes

Settings for [geerlingguy.certbot](https://github.com/geerlingguy/ansible-role-certbot)

    # For live certs switch this value
    # certbot_cloudflare_acme_server: "{{ certbot_cloudflare_acme_live }}"
    certbot_cloudflare_acme_server: "{{ certbot_cloudflare_acme_test }}"
    # Email address for Cloudflare Global API Key
    certbot_cloudflare_email: "cloudflare@example.com"
    # Create varable using.
    # ansible-vault encrypt_string 'cloudflareAPIKey' --name 'certbot_cloudflare_api_key'
    certbot_cloudflare_api_key: ''

Settings for [michaelpporter.certbot_cloudflare](https://github.com/geerlingguy/ansible-role-certbot)

    # Jenkins Playbook
    jenkins_vhost: "demojenkins.example.com"
    jenkins_email: "jenkins@example.com"
    jenkins_admin_username: admin
    # ansible-vault encrypt_string 'demo' --name 'jenkins_admin_password'
    jenkins_admin_password: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            33303334313764363663333839313035393365313435303066663735633530336562313964663361
            3630396634653534393236336630626336623834646564610a646538376465313137303462386330
            33666666656438343563313130353066376330366135626565373033326437386538653431393264
            6131663466666165640a366430376165316362376538613162623838386639323565613961626532
            39313762343261646362313533386136636339636535346131656362323234323664633965633836
            32396265623037376465663030373237336136636631383433393238616636373339623032653031
            31333632343365373337363332623234663838643435323263386535623639373263326465383234
            38646135616265323366326134323737383365336331353939633837346465333030383032323461
            36353632393065386462383033323334333931353138316262333364393961333037

Settings for [geerlingguy.jenkins](https://github.com/geerlingguy/ansible-role-jenkins)

    config_htaccess_user: "demo"
    # Encrypt htaccess password
    ## ansible-vault encrypt_string 'demo' --name 'config_htaccess_pass'
    config_htaccess_pass: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            66353333343463626535333430323630353639653939366634656130383463333861616365656265
            3562666238643931623738383566353138366633656637610a613934366136306563303438303462
            63303766333661643930623663373631386234626161656238643536313639643535363463306237
            6261613334633236630a636636636364323165663361343936623333333265616537353966356566
            3837

Set htaccess key to protect staging server.

    # base domain for webgroup hosts URL will become https://folder.webgroup01.example.com
    base_webgroup_domain: "example.com"

Base domain for servers

    # if you use gmail/gsuite and want to setup postfix as a relay set this to true
    config_postfix_use_gmail: false
    config_postfix_gmail_address: "devops@gmail.com"
    ## ansible-vault encrypt_string 'gmailpass' --name 'config_postfix_gmail_pass'
    config_postfix_gmail_pass: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            64396262356336636430313233333962383739356537343536653761306439633465633033303136
            3166353335343662303130386239393164663962626263360a333362316465623934633563383866
            39326363626433353232656331663764373633353031353063636137666663663564643038636638
            3633306637386131310a366131363262343837356437386665646139626364653036323130326338
            6261
    # Allowed domains or address to send to, all others will be dropped.
    config_postfix_allowed: []
    # - example.com
    # - devops@gmail.com

Postfix settings.

    ## Pantheon CI https://pantheon.io/docs/guides/jenkins/
    config_terminus_ci: false

Installs tools for Pantheon.

    ## ansible-vault encrypt_string 'rootpassword' --name 'mysql_root_password'
    mysql_root_password: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            37373531363432363033623733353839663863336662373639313866393966396661366535393639
            3435623535613566316361356165373465396432316539620a666666636165613834323734353064
            30306261346637313839636162666638613733636534333263653361656163346139653661653265
            3165333533633764610a643730393364323234636435313661363761363736623664313263613064
            3036

Set mySQL root password.

    setup_jenkins_url: "{{jenkins_vhost}}"
    setup_jenkins_github_deploy: "global-builds/pr-deploy/"
    ## ansible-vault encrypt_string 'keyFromJenkinsJob' --name 'setup_jenkins_github_deploy_key'
    setup_jenkins_github_deploy_key: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            66366138306639323366386433343861623964653934656564663730636566363139646161343732
            6133663237653938333764396433393132306137643630350a336330623633333166613836623839
            32336338626530343134656566373339386239333063353062653337386265333066353335363638
            3837306538616464620a656436336165323232336265313965346639326537383531316664656535
            65613066353233313666386231316336353838356239336338373063396638363565
    setup_jenkins_clean_job: "global-builds/clean-sites/"
    ## ansible-vault encrypt_string 'keyFromJenkinsJob' --name 'setup_jenkins_clean_job_key'
    setup_jenkins_clean_job_key: !vault |
            $ANSIBLE_VAULT;1.1;AES256
            66363937303332353064646539373763356466663732643666633466313236646534346462646237
            3135306431616139616339313866643037306663626362360a643162616661613337333038373839
            34323238333135363265336537313331643338623661666535323437303930393466643562666435
            6661316263386138340a613534323235376334656130323235356338613766316130366232346135
            3262
    setup_servers: "{{ groups['webgroup'] }}"
    ## ansible-vault encrypt_string 'slackAPIKey' --name 'setup_slack_api'
    setup_slack_api: ''

Variables for the cleanup and deploy jobs, called from GitHub webhooks.

    # Logwatch Playbook
    logwatch_send_to: "mailto@example.com"
    logwatch_send_from: "mailfrom@example.com"

Email addresses to send Logwatch reports to.

    # Composer
    composer_home_path: '/home/git/.composer'
    composer_home_owner: git
    composer_home_group: git
    composer_global_packages:
    - { name: phpunit/phpunit, release: "4.7.*" }
    - { name: pantheon-systems/terminus, release: "^1.8" }
    - { name: squizlabs/php_codesniffer, release: "^2.9.1" }
    - { name: phploc/phploc, release: "*" }
    composer_keep_updated: true
    composer_add_to_path: true

Composer settings and global install.

    # Drush
    drush_launcher_install: no
    drush_install_from_source: yes
    drush_source_install_bin_path: /usr/local/bin/drush
    drush_source_install_path: /usr/local/share/drush
    drush_source_install_version: "8.x"
    drush_keep_updated: yes
    drush_force_update: no

Install drush 8 globally.

    # swap
    swapfile_size: 1GB
    my_custom_path_var: /usr/local/scripts

Swap file settings

    setup_apt_install:
    - aptitude
    - build-essential
    - cpanminus
    - curl
    - imagemagick
    - libcache-cache-perl
    - libdbd-mysql-perl
    - libdbi-perl
    - libffi-dev
    - libssl-dev
    - librose-datetime-perl
    - libtext-csv-perl
    - libtext-csv-xs-perl
    - ntp
    - perl
    - php-pear
    - python-apt
    - python-dev
    - python-pycurl
    - ruby-dev
    - s3cmd
    - sshpass
    - unzip

Apps to install, these are required, feel free to add to them.

## Commands

### First run

```shell
ansible-galaxy install -r requirements.yml -f
ansible-playbook setup.yml
ansible-playbook main.yml
```

### 2-N runs

```shell
ansible-playbook main.yml
```

### Adding a new webserver; i.e. web30

```shell
ansible-playbook setup.yml --limit=web03
ansible-playbook main.yml
```

## Local ssh config

Setup your SSH config file to include the servers you are working with `~/.ssh/config`.

```shell
Host jenkins
    Hostname remoteip # Remote IP Address
    User demo # `user_sudouser_username` from var/main-vars.yml
    Port 7822 # `remote_port` from var/main-vars.yml

Host web01
    Hostname remoteip
    User demo
    Port 7822

Host web02
    Hostname remoteip
    User demo
    Port 7822
```

## Ansible Hosts file

Edit your hosts file to have a `jenkinsgroup`, `webgroup` and `demogroup`, if you use different names for the groups you can do a find and replace in the files.

To find the path for the invitory file:

```shell
ansible --version
ansible 2.6.3
  config file = /Users/username/.ansible.cfg
  configured module search path = [u'/Users/username/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/Cellar/ansible/2.6.3/libexec/lib/python2.7/site-packages/ansible
  executable location = /usr/local/bin/ansible
  python version = 2.7.15 (default, Jul 23 2018, 21:27:06) [GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.2)]

grep --color=auto -iR 'inventory' ~/.ansible.cfg
/Users/username/.ansible.cfg:#inventory      = /etc/ansible/hosts
/Users/username/.ansible.cfg:inventory = ~/.ansible/hosts
```

```ini
[jenkinsgroup]
jenkins

[webgroup]
web01 php_version='7.1'
web02 php_version='7.1'

[demogroup:children]
jenkinsgroup
webgroup
```

If your `jenkins` host is has anohter name, rename `var/jenkins.yml` to match. i.e. `jenkins-dev` `var/jenkins-dev.yml`

```ini
[jenkinsgroup]
jenkins-dev
```
