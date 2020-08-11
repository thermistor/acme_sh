![Current tag](https://img.shields.io/github/tag/thermistor/acme_sh.svg)

# acme_sh

Setup acme.sh in dns api mode via ansible to generate letsencrypt.org certs.

## Dependencies

A lot of defaults assume using nginx.

## Vars

Here are some defaults that control behavior:

    acme_sh_autoupgrade: True
    acme_sh_notify: False
    acme_sh_logging: False
    acme_sh_keylength: 4096
    acme_sh_dns_sleep: 120

    acme_sh_certs_public_dir: /etc/nginx/certs
    acme_sh_certs_private_dir: /etc/nginx/private
    acme_sh_reload_cmd: sudo /bin/systemctl reload nginx

## Example Playbook

Here is an example using [AWS Route53 API](https://github.com/Neilpang/acme.sh/wiki/How-to-use-Amazon-Route53-API). But you could modify it for any dns api provider.

Usage:

    - hosts: servers
      roles:
        - role: thermistor.acme_sh
          acme_sh_subject_names:
            - example.com
            - www.example.com
          acme_sh_dns_provider: dns_aws
          acme_sh_env:
            AWS_ACCESS_KEY_ID: "{{ your_vault_aws_access_key_id }}"
            AWS_SECRET_ACCESS_KEY: "{{ your_vault_aws_secret_access_key }}"

here is the same example but with logging added:

    - hosts: servers
      roles:
        - role: thermistor.acme_sh
          acme_sh_logging: True
          acme_sh_subject_names:
            - example.com
            - www.example.com
          acme_sh_dns_provider: dns_aws
          acme_sh_env:
            AWS_ACCESS_KEY_ID: "{{ your_vault_aws_access_key_id }}"
            AWS_SECRET_ACCESS_KEY: "{{ your_vault_aws_secret_access_key }}"

and with mailgun notifications:

    - hosts: servers
      roles:
        - role: thermistor.acme_sh
          acme_sh_notify: True
          acme_sh_notify_hooks:
            - mailgun
          acme_sh_subject_names:
            - example.com
            - www.example.com
          acme_sh_dns_provider: dns_aws
          acme_sh_env:
            AWS_ACCESS_KEY_ID: "{{ your_vault_aws_access_key_id }}"
            AWS_SECRET_ACCESS_KEY: "{{ your_vault_aws_secret_access_key }}"
            MAILGUN_API_KEY: "{{ your_vault_mailgun_api_key }}"
            MAILGUN_API_DOMAIN: "{{ your_vault_mailgun_domain }}"
            MAILGUN_FROM: "{{ your_vault_mailgun_from }}"
            MAILGUN_TO: "{{ your_vault_mailgun_to }}"

## What happens under the hood

If you generate a cert for example.com, during the install step this role will copy the `/var/lib/acme/.acme.sh/example.com/fullchain.cer` and install it as `/etc/nginx/certs/example.com.cer`. Note that it is installing the **fullchain**
 cert and renaming it, this is so that you can install multiple fullchain certs for different domains if necessary.

## Troubleshooting

If there are cert misconfiguration issues sometimes you can get into a wedged state where certs are not being reinstalled, you can force the certs to be reinstalled with:

    ansible-playbook -i inventory playbook.yml -e "acme_sh_force_install=True" --tags acme_sh_cert_install

## License

MIT

## Alternatives

We borrowed a lot from these alternatives:

* [nickjj.acme_sh](https://galaxy.ansible.com/nickjj/acme_sh) - multi cert capable
* [verosk.acme-sh](https://galaxy.ansible.com/verosk/acme-sh) - just install
