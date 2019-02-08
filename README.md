# acme_sh

Setup acme.sh via ansible to generate letsencrypt.org certs.

## Example Playbook

Usage:

    - hosts: servers
      roles:
        - role: thermistor.acme_sh
          acme_sh_domains: ['example.com', 'www.example.com']

## License

MIT

## Alternatives

* [verosk.acme-sh](https://galaxy.ansible.com/verosk/acme-sh)
