FROM php:7.4.2-apache

ADD https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz .
RUN tar xvf dokuwiki-stable.tgz && rm dokuwiki-stable.tgz && mv dokuwiki* dokuwiki && chown -R www-data:www-data dokuwiki
