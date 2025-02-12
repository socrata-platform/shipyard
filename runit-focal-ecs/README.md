socrata/runit-focal:ecs
============

Image based on phusion/baseimage-docker that establises a base set of patterns and tools for building other containers with support for multiple processes via runit.

### Features

- Ubuntu 20.04 LTS. The base system.
- A correct init process (my_init) with
  - proper process reaping
  - Docker Stop sends SIGTERM to the init process, which stops all processes gracefully on termination.
- runit	replaces Ubuntu's Upstart. Used for service supervision and management
- syslog-ng: A syslog daemon is necessary so that many services - including the kernel itself - can correctly log to /var/log/syslog
- logrotate	Rotates and compresses logs on a regular basis.
- SSH server: disabled by default
- cron	The cron daemon must be running for cron jobs to work.
- setuser	A tool for running a command as another user. Easier to use than su, has a smaller attack vector than sudo, and unlike chpst this tool sets $HOME correctly. Available as /sbin/setuser.

See the usage section below

Inside a docker container, the host’s ip and name are not available.  By invoking these scripts, we can make the container aware of its host as necessary.  They create the environment variables ARK_HOST and ARK_HOSTNAME.

- env_parse:
A tool for generating config files from [jinja](http://jinja.pocoo.org/) templates and environment variables.

This script lets a service owner build configuration files that are created at run time from environment variables and a template file.  The template language is jinja and any template variables must be available as environment variables.  env_parse takes one argument, the template, and an optional argument for the output file.  If omitted, the output file is identical to the template file name with the trailing .j2 removed.

### Usage

Any container built on top of the socrata/runit-focal image will default to running the whatever services are configured in /etc/service according via runit.

Anything placed in /etc/my_init.d will be run on startup in lexigraphical order before runit is invoked. A non-zero return code from any of these will halt the container.

### Example

Assuming we build an image called awesome_sauce from a Dockerfile like this:

```Dockerfile
    FROM socrata/runit-focal

    RUN mkdir /etc/service/myservice
    COPY myservice-run /etc/service/myservice/run
    COPY myservice-log /etc/service/myservice/log/run
```

Where `run` and `log` are runit service definitions where the `run` script looks like:

```bash
    #!/bin/sh
    exec /sbin/setuser socrata my_binary
```

and the `log` script looks like:

```bash
    #!/bin/sh
    exec svlogd -tt /var/log/myservice
```
