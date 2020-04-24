NGINX Controller Application
==========================

Define an application with NGINX Controller to hold components that describe the behavior to the upstream workload group(s).

Requirements
------------

[NGINX Controller](https://www.nginx.com/products/nginx-controller/)

Role Variables
--------------

### Required Variables

`controller.fqdn` - FQDN of the NGINX Controller instance

`controller.auth_token` - Authentication token for NGINX Controller

`environmentName` - Environment the component is associated with

`application.metadata.name` -  Name of the application

### Template Variables

This role has multiple template related variables. The descriptions and defaults for all these variables can be found in **[vars/main.yml](./vars/main.yml)**

Dependencies
------------

Example Playbook
----------------

To use this role you can create a playbook such as the following (let's name it `nginx_controller_application.yaml` for the purposes of this example).

```yaml
- hosts: localhost
  gather_facts: no

  vars:
    controller:
      user_email: "user@example.com"
      user_password: "mySecurePassword"
      fqdn: "controller.mydomain.com"
      validate_certs: false

  tasks:
    - name: Retrieve the NGINX Controller auth token
      include_role:
        name: nginxinc.nginx_controller_generate_token

    - name: Configure the application
      include_role:
        name: nginxinc.nginx_controller_application
      vars:
        environmentName: "production-us-west"
        app:
          metadata:
            name: "trading.acmefinancial.net"
            displayName: "Trading.ACMEFinancial.net"
            description: "Core Trading Application"
```

You can then run `ansible-playbook nginx_controller_application.yaml` to execute the playbook.

Alternatively, you can also pass/override any variables at run time using the `--extra-vars` or `-e` flag like so `ansible-playbook nginx_controller_application.yaml -e '{"controller":{ "user_email":"user@company.com","user_password":"notsecure", "fqdn": "controller.example.local", "validate_certs":false }}'`

You can also pass/override any variables by passing a `yaml` file containing any number of variables like so `ansible-playbook nginx_controller_application.yaml -e "@nginx_controller_application_vars.yaml"`

License
-------

[Apache License, Version 2.0](./LICENSE)

Author Information
------------------

[Brian Ehlert](https://github.com/brianehlert)

&copy; [NGINX, Inc.](https://www.nginx.com/) 2020
