# nginx_reverse_proxy
A basic nginx reverse proxy configured to be the central access point to several other websites. Such as:
* dokuwiki
* wekan
* freshrss

This proxy is configured with TLS client certificates to ensure only authorised people have access to the websites. It also simplifies the configurations for use over an SSH Tunnel.
