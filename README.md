# Configuration Management Tool for PHP Web application

A rudimentary configuration management tool and use it to configure servers for production service of a simple PHP web application. Do not use off-the-shelf tools like (but not limited to) Puppet, Chef, Fabric, or Ansible. Instead, implement a tool to meet the following specifications and then use that tool to configure the two servers.

Requirements for the rudimentary configuration management tool:
1. If the tool has dependencies not available on a standard Ubuntu instance you may include a bootstrap.sh program to resolve them
2. The tool must provide an abstraction that allows specifying a file's content and metadata (owner, group, mode)
3. The tool must provide an abstraction that allows installing and removing Debian packages
4. The tool must provide some mechanism for restarting a service when relevant files or packages are updated
5. The tool must be idempotent - it must be safe to apply the configuration over and over again
6. The toil must specify a web server capable of running the PHP application below
```
<?php
header("Content-Type: text/plain");
echo "Hello, world!\n";
?>
```
* The server must respond 200 OK and include the string "Hello, world!" in their response to requests from curl -sv http://ip-address (public)


# Proposal

- initial setup, create SSH keys for further usage
- similar to Ansible, apply bootstrap and update target server via SSH
- major three commands: init -> apply -> status


# Code structure

```
├── LICENSE
├── README.md
├── sscmt
└── templates
    ├── php.bootstrap
    ├── php.homepage
    └── php.meta
```


# Usage

```
./sscmt <command> <target> <params>

Examples:
./sscmt init web-server1
./sscmt update web-server1
./sscmt status web-server1 "appY"
./sscmt add-package web-server1 "appX appY appZ"
./sscmt remove-package web-server1 "appY"
```

# Initial setup

```
./sscmt init <target>  # target: a unique name of server
./sscmt apply <target>
```
Note: SSH keys and meta data saved under ~/.sscmt/


# Change page content and Deploy

modify homepage under templates/php.homepage, run:
```
./sscmt apply <target>

```

# Modify meta or bootstrap

modify bootstrap or meta configuration under templates, run:
```
./sscmt init <target>  # target: a unique name of server
./sscmt apply <target>
``````

# Check status

run `./sscmt status`


# TODO

- apply to all targets together in one run
- support different languages
