# acme-dns-pdns

This is the PowerDNS-compatible server part of [acme-dns-client](https://github.com/stuvusIT/acme-dns-client).
It expects to be run behind a reverse proxy which verifies the certificates and sets the client certificate DN into a header.

The ACL system works by allowing a DN (key of the dict) to request ceritificates for a list of names (the value) or a list consisting of the single element `*` which allows fetching certificates for all names.

After the record was changed, the secondaries are notified and the server waits for replication.

## Requirements

Debian 11

## Role Variables

| Name                      | Default / Mandatory | Description                                                                                          |
|:--------------------------|:-------------------:|:-----------------------------------------------------------------------------------------------------|
| `acme_server_listen_host` | `127.0.0.1`         | The host to listen on                                                                                |
| `acme_server_listen_port` | `8000`              | The port to listen on                                                                                |
| `acme_server_dn_header`   | `X-Client-DN`       | The header containing the DN of the client                                                           |
| `acme_server_pdns_host`   | `127.0.0.1:8053`    | The host and port where the PowerDNS API resides                                                     |
| `acme_server_dig_host`    | `127.0.0.1`         | The host to ask the initial dig question for verifying whether the record is successfully replicated |
| `acme_server_acls`        | `{}`                | DN-Hostnames mapping of ACLs for this server                                                         |

## Example Playbook

```yml
- hosts: dns
  roles:
    - role: acme-dns-pdns
      acme_server_acls:
        CN=imap.example.com:
          - imap.example.com
          - *.imap.example.com
        CN=anotherhost.example.com:
          - '*'
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

## Author Information

- [Janne Heß](https://github.com/dasJ)
