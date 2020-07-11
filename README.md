# Pi-hole

Role to deploy and configures everything required to run Pi-hole on a Raspberry Pi in your network. Based off of the twinsnes.pihole role

## Requirements

* Ansible 2.9 or newer

## Role Variables


| variable name                   | default     | description                                                                                                                                                                                                             |
|---------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `raspberry_user`                | pi          | The user that will be running Pi-hole.                                                                                                                                                                                  |
| `dns_server1`                   | 8.8.8.8     | The IP for the primary DNS server Pi-hole will forward requests to.                                                                                                                                                     |
| `dns_server2`                   | 8.8.4.4     | The IP for the secondary DNS server Pi-hole will forward request to if the primary DNS server is not available. <br/><br/> Use the value "no" to  disable the secondary DNS                                             |
| `pihole_admin_password`         | random      | Administrator password for the web ui for Pi-hole. <br/><br/> It is recommended that you set this to something else, or you will have to dig through the logs on the host to find the automatically generated password. |
| `pihole_dnsmasq_templates`      | []          | A list of src/name template files that will be installed to configure dnsmasq. Useful if you have multiple subnets you want to mask out, for example                                                                    |
| `pihole_conditional_forwarding` | `undefined` | A mapping with router_ip and domain that enables the default conditional forwarding in the pihole                                                                                                                       |


## Dependencies

* geerlingguy.docker
* geerlingguy.pip

## How to use

First install the role on your management node / local machine, which should download all the required dependencies. 

```shell
ansible-galaxy install offbyone.pihole
```

Then either add the role with required configuration to your playbook for your Pi, or make a new one like the one below.

Playbook file:

```yaml
- hosts: all
  roles:
  - name: offbyone.pihole
    vars:
      pihole_admin_password: "SomeSecretPassword"
```

Using an inventory file makes it a lot easier to manage hosts and will alow you to store configuration for your host in a file for later. So when you need to run this again in 6 months, you don't have to remember the settings.

Inventory file:

```yaml
all:
  vars:
    ansible_connection: ssh
    ansible_ssh_user: pi
    ansible_ssh_pass: raspberry
  hosts:
    raspberry:
      ansible_host: "192.168.1.100"
```

Once you have both your playbook and your inventory file set up. Execute your playbook

```shell
ansible-playbook playbook.yaml -i inventory.yaml
```

## License

Apache-2.0
