Certgrinder client role
========================

The following settings are needed:

certgrinder_ssh_server: "certgrinder.servers.example.org"
certgrinder_redirect_url: "https://certgrinder.example.org"
certgrinder_hostname_sets:
  domainlist:
    - "example.com,www.example.com"
    - "example.org,www.example.org"

Optionally certgrinder can restart local services after a cert is renewed:

certgrinder_post_renew_hooks:
  post_renew_hooks:
    - "/usr/sbin/service -R"

Something to handle the challenge HTTP redirect is also needed. nginx_server role can be used with something like the following nginx_locationslash:

nginx_locationslash:
  location /.well-known/acme-challenge/ {
        return 301 {{ certgrinder_redirect_url }}$request_uri;
  }
  location / {
        return 301 https://example.com;
  }

... or any other way. Or just redirect it in the firewall.
