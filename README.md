# Ansible playbook: Bootstrap PowerDNS recursor on EC2

This playbook is intended to be called by a user-data script when provisioning an EC2 instance. This will provision a private PDNS recursor you can point to for DNS resolution. To black hole specific domains from look-ups, add them to `/etc/powerdns/blocklist`.

If you have other domains hosted on Route53, then you can add them to the forward zones, which allows you to resolve your R53 records through the PDNS recursor.


## Installation

This playbook is designed to be run in a user-data script , but if you want to run this manually you can:

  1. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html).
  2. Clone this repository to your EC2 instance
  3. `# ansible-galaxy install -r requirements.yml`
  4. `# ansible-playbook site.yml --extra-vars "@~/extra-vars.json"`


`~/extra-vars.json` contains the Terraform variables specifically for the [Ansible Wireguard role](https://github.com/gavmain/ansible-role-pdns-recursor) into the Ansible playbook. Example:

    {
        "listen_address": "0.0.0.0",
        "forward_zones": [
            {
                "internal.example.com": {
                    "ips": [
                        "192.168.1.53"
                    ]
                }
            },
            {
                ".": {
                    "ips": [
                        "1.1.1.1",
                        "8.8.8.8"
                    ]
                }
            }
        ]
    }
## Author

[Gav Main](https://github.com/gavmain), 2023.
