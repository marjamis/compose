# nginx_reverse_proxy
A basic nginx reverse proxy configured to be the central access point to several other websites. Such as:
* dokuwiki
* wekan

It also simplifies the use of SSH Tunneling from a trusted publicly accessible endpoint.

This proxy is configured with TLS client certificates to also ensure only authorised people have access to the websites.
