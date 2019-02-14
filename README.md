![Current tag](https://img.shields.io/github/tag/thermistor/acme_sh.svg)

# acme_sh

Setup acme.sh in dns api mode via ansible to generate letsencrypt.org certs.

## Dependencies

A lot of defaults assume using nginx.

## Vars

Can tune a few parameters, here are the defaults:

    acme_sh_autoupgrade: True
    acme_sh_keylength: 4096
    acme_sh_dns_sleep: 120

    acme_sh_certs_public_dir: /etc/nginx/certs
    acme_sh_certs_private_dir: /etc/nginx/private
    acme_sh_reload_cmd: /bin/systemctl reload nginx

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
          acme_sh_dns_api_keys:
            AWS_ACCESS_KEY_ID: "{{ your_vault_aws_access_key_id }}"
            AWS_SECRET_ACCESS_KEY: "{{ your_vault_aws_secret_access_key }}"

## License

MIT

## Alternatives

We borrowed a lot from these alternatives:

* [nickjj.acme_sh](https://galaxy.ansible.com/nickjj/acme_sh) - multi cert capable
* [verosk.acme-sh](https://galaxy.ansible.com/verosk/acme-sh) - just install
