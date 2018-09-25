# hub.docker.com/r/tiredofit/nginx-ldap

[![Build Status](https://img.shields.io/docker/build/tiredofit/nginx-ldap.svg)](https://hub.docker.com/r/tiredofit/nginx-ldap)
[![Docker Pulls](https://img.shields.io/docker/pulls/tiredofit/nginx-ldap.svg)](https://hub.docker.com/r/tiredofit/nginx-ldap)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/nginx-ldap.svg)](https://hub.docker.com/r/tiredofit/nginx-ldap)
[![Docker 
Layers](https://images.microbadger.com/badges/image/tiredofit/nginx-ldap.svg)](https://microbadger.com/images/tiredofit/nginx-ldap)

# Introduction

This will build a container for [Nginx](https://www.nginx.org) w/ LDAP Authentication Enabled

*    Tracks Mainline release channel
*    Includes Zabbix Monitoring (nginx status) on port 73
*    Logrotate Included to roll over log files at 23:59, compress and retain for 7 days
*    Ability to Password Protect (Basic) or use LemonLDAP:NG Handler
*    Compile Options:
*    --with-threads
        --with-http_ssl_module 
        --with-http_realip_module 
        --with-http_addition_module 
        --with-http_sub_module 
        --with-http_dav_module 
        --with-http_flv_module 
        --with-http_mp4_module 
        --with-http_gunzip_module 
        --with-http_gzip_static_module 
        --with-http_random_index_module 
        --with-http_secure_link_module 
        --with-http_stub_status_module 
        --with-http_auth_request_module 
        --with-http_xslt_module=dynamic 
        --with-http_image_filter_module=dynamic 
        --with-http_geoip_module=dynamic 
        --with-http_perl_module=dynamic 
        --with-threads 
        --with-stream 
        --with-stream_ssl_module 
        --with-stream_ssl_preread_module 
        --with-stream_realip_module 
        --with-stream_geoip_module=dynamic 
        --with-http_slice_module 
        --with-mail 
        --with-mail_ssl_module 
        --with-compat 
        --with-file-aio 
        --with-http_v2_module 
        
This Container uses [tiredofit:alpine:3.7](https://hub.docker.com/r/tiredofit/alpine) as a base.


[Changelog](CHANGELOG.md)

# Authors

- [Dave Conroy](https://github.com/tiredofit)

# Table of Contents

- [Introduction](#introduction)
    - [Changelog](CHANGELOG.md)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
    - [Data Volumes](#data-volumes)
    - [Environment Variables](#environmentvariables)   
    - [Networking](#networking)
- [Maintenance](#maintenance)
    - [Shell Access](#shell-access)
   - [References](#references)

# Prerequisites

This image assumes that you are using a reverse proxy such as [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) and optionally the [Let's Encrypt Proxy Companion @ https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) in order to serve your pages. However, it will run just fine on it's own if you map appropriate ports.


# Installation

Automated builds of the image are available on [Docker Hub](https://hub.docker.com/tiredofit/nginx-ldap) and is the recommended method of installation.


```bash
docker pull tiredofit/nginx-ldap
```

# Quick Start

* The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [docker-compose.yml](examples/docker-compose.yml) that can be modified for development or production use.

* Set various [environment variables](#environment-variables) to understand the capabilities of this image.
* Map [persistent storage](#data-volumes) for access to configuration and data files for backup.
* Make [networking ports](#networking) available for public access if necessary



# Configuration

### Data-Volumes

The container starts up and reads from `/etc/nginx/nginx.conf` for some basic configuration and to listen on port 73 internally for Nginx Status responses. `/etc/nginx/conf.d` contains a sample configuration file that can be used to customize a nginx server block. The LDAP configuration resides in the `/etc/nginx/conf.d/01-ldap.conf` upon container start.


The following directories are used for configuration and can be mapped for persistent storage.

| Directory    | Description                                                 |
|--------------|-------------------------------------------------------------|
|  `/www/html` | Drop your Datafiles in this directory to be served by Nginx |
|  `/www/logs` | Logfiles for Nginx error and access                         |
      

### Environment Variables

Along with the Environment Variables from the [Base image](https://hub.docker.com/r/tiredofit/alpine), below is the complete list of available options that can be used to customize your installation.

Authentication Options

| Parameter | Description |
|-----------|-------------|
| `AUTHENTICATION_TYPE` | Protect site - `NONE`,`BASIC`,`LLNG` - Default `NONE` |
| `WEB_USER` | If `BASIC` chosen enter this for the username to protect site |
| `WEB_PASS` | If `BASIC` chosen enter this for the password to protect site |
| `LLNG_HANDLER_HOST` | If `LLNG` chosen use hostname of handler - Default `llng-handler`
| `LLNG_HANDLER_PORT` | If `LLNG` chosen use this port for handler - Default `2884` |

The `LLNG` option is for when using LemonLDAP:NG Handlers to protect your application and require modification to the `/etc/nginx/conf.d/default.llng` file to fully work properly! 

General Options 


| Parameter        | Description                            |
|------------------|----------------------------------------|
| `UPLOAD_MAX_SIZE` | Maximum Upload Size for Nginx (e.g 2G) |
| `LDAP_HOST` | Hostname and port number of LDAP Server (e.g. ldapserver:389) |
| `LDAP_BIND_DN` | User to Bind to LDAP (e.g. cn=admin,dc=orgname,dc=org) |
| `LDAP_BIND_PW` | Password for Above Bind User (e.g. password) |
| `LDAP_BASE_DN` | Base Distringuished Name (e =dc=hostname,dc=com |
| `LDAP_ATTRIBUTE` | Unique Identifier Attrbiute (e.g. uid) |
| `LDAP_SCOPE` |LDAP Scope for searching (e.g. sub) |
| `LDAP_FILTER` | Define what object that is searched for (e.g. objectClass=person) |
| `LDAP_GROUP_ATTRIBUTE` | If searching inside of a group what is the Group Attribute (e.g. uniquemember) |



### Networking

The following ports are exposed.

| Port      | Description |
|-----------|-------------|
| `80`      | HTTP        |
| `443`     | HTTPS       |


# Maintenance
#### Shell Access

For debugging and maintenance purposes you may want access the containers shell. 

```bash
docker exec -it (whatever your container name is e.g. nginx-ldap) bash
```

# References

* https://nginx.org/
* https://github.com/kvspb/nginx-auth-ldap
