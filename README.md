# acme

Setup acme.sh via ansible to generate letsencrypt.org certs.

## Example Playbook

Usage:

    - hosts: servers
      roles:
        - role: thermistor.acme
          domains: ['example.com', 'www.example.com']

## License

MIT

## Alternatives

* [verosk.acme-sh](https://galaxy.ansible.com/verosk/acme-sh)
