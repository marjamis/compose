# nginx_reverse_proxy
A basic nginx reverse proxy configured to be the central access point to several other websites. Such as:
* dokuwiki
* owncloud
* wekan

It also simplifies the use of SSH Tunneling from a trusted publicly accessible endpoint.

This proxy is(or more appropriately will be) configured with TLS client certificates to also ensure only authorised people have access to the websites.
