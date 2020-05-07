# Metodología HTB

## 1 - Comprobar ping en la máquina.

```console
security@beginners:~$ ping -c 1 IP
```
Comprobamos el TTL de la respuesta. Normalmente si es 128 será Windows o 64 Linux. 

| OS            | TTL           | TCP Windows Size |
|:-------------:|:-------------:| :---------------:|
| Linux         |64             | 5840             |
| FreeBSD       |54            |65535             |
| Windows       |128            |65535             |
| Cisco Router  |255           |4128              |

## Servicios activos / puertos abiertos

```console
security@beginners:~$ nmap -p- -T5 -v -n ip -oG allPorts
```