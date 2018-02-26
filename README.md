# Overview
This playbook creates Docker container with Postfix, configured to relay e-mails between your address on a custom domain and a private one (e.g. GMail).
It is mostly based on these tutorials:
- https://seasonofcode.com/posts/custom-domain-e-mails-with-postfix-and-gmail-the-missing-tutorial.html
- https://seasonofcode.com/posts/setting-up-dkim-and-srs-in-postfix.html,

so please make yourself familiar with the configuration process.
Also, please note this playbook assumes your Docker daemon is configured to accept TLS-protected connections from remote clients; there is a great doc page about setting it up: https://docs.docker.com/engine/security/https/.

# Usage
1. Clone this repo.
2. Change the following variables in a proper way [1]:
    - **remote_host** is where your Docker daemon runs;
    - **username** is **this**@part.of.your.custom.email;
    - **domain** is the@**other.one**;
    - **forward_email** is where you want your precious letters to have their final destination;
    - **country** is ISO Alpha-2 country code (e.g. AQ) used for certificate generation;
    - **organization** is used for the same purpose, fill it as you wish, it doesn't really matter;
    - **sasl2_password**: we need some authentication facilities because "open relays are a terrible idea" [[2]](https://seasonofcode.com/posts/custom-domain-e-mails-with-postfix-and-gmail-the-missing-tutorial.html)
3. Edit *hosts* as follows:
```
[target_machine]
your_server_address
```
4. Run ansible:
```
ansible-playbook -i hosts deploy.yml
```
5. Change your DNS records appropriately (this playbook prints DKIM DNS record as a debug message).
6. Set up your GMail account to send messages via your new mail server.

[1]: If you are curious about the garbage-looking default values of these variables, see [this doc](http://docs.ansible.com/ansible/2.4/vault.html).
