# Ansible Role - radiorabe.certbot.certbot

Install Certbot, enable and start service and add script to push changes to another host using [`ansible.builtin.package`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html), [`ansible.builtin.systemd_service`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/systemd_service_module.html) and [`ansible.builtin.template`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html).

## Requirements

None.

## Role Variables

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `certbot_acme_acount_mail` | `certbot@rabe.ch` | certbot acme account mail address for certificate expiration notifications. |
| `certbot_certbot_package_name` | `certbot-renew` | certbot package name in repository. |
| `certbot_certbot_binary_path` | `/usr/bin/certbot` | path to certbot binary. |
| `certbot_certbot_sysconfig_path` | `/etc/sysconfig/certbot` | path to certbot sysconfig for deploy hook which runs `certbot_certsync_script_path`. |
| `certbot_certsync_script_path` | `/usr/local/libexec/cert_sync.sh` | path to script which synchronizes certificates to remote host. |
| `certbot_rsync_package_name` | `rsync` | rsync package name in repository. |
| `certbot_certbot_systemd_timer_name` | `certbot.timer` | name of certbot systemd timer for certificate renewals. |
| `certbot_certificates_src` | `/etc/letsencrypt/live/` | source directory of issued certificates and keys. |
| `certbot_ceriticates_dest` | `/home/{certbot_remot_user}/httpd/rabe_certs` | destination of certificates and keys on remote host. |
| `certbot_certificates` | `letest.rabe.ch, letest2.rabe.ch` | comma separated list of certificates managed by certbot |
| `certbot_remote_hosts` | `[ "localhost" ]` | list of remote hosts requiring issued certificates and keys. |
| `certbot_remote_user` | `revproxy` | remote user. |
| `certbot_remote_container` | `false` | set to true if a container needs signal (`certbot_remote_container_signal`) for reading new certificates. |
| `certbot_remote_container_name` | `revproxy-revproxy` | name of container to send signal. Requires `certbo_remote_container` be set to `true`. |
| `certbot_remote_container_signal` | `USR1` | signal to send to container. Requires `certbo_remote_container` be set to `true`. |
| `certbot_remote_container_runtime_path` | `/usr/bin/podman` | path container runtime (should work with `docker` as well). |
| `certbot_remote_rsync_binary_path` | `/usr/bin/rsync` | path to rsync binary. |

## Dependencies

None

## Example Playbook

```yaml
- hosts: all
  roles:
    - certbot
      vars:
        - certbot_acme_account_mail: certbot@rabe.ch
          certbot_certbot_package_name: certbot
          certbot_certbot_binary_path: /usr/bin/certbot
          certbot_certbot_sysconfig_path: /etc/sysconfig/certbot
          certbot_certsync_script_path: /usr/local/libexec/cert_sync.sh
          certbot_rsync_package_name: rsync
          certbot_certbot_systemd_timer_name: certbot-renew.timer
          certbot_certificates_src: /etc/letsencrypt/live/
          certbot_ceriticates_dest: /home/{certbot_remot_user}/httpd/rabe_certs
          certbot_certificates: letest.rabe.ch,letest2.rabe.ch
          certbot_remote_hosts: [ "localhost" ]
          certbot_remote_user: revproxy
          certbot_remote_container: true
          certbot_remote_container_name: revproxy-revproxy
          certbot_remote_container_signal: USR1
          certbot_remote_container_runtime_path: /usr/bin/podman
          certbot_rsync_binary_path: /usr/bin/rsync

```

## License

This role is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, version 3 of the License.
