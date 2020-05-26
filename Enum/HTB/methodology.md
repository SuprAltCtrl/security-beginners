# Metodología HTB

## 1 - Comprobar ping en la máquina

```console
security@beginners:~$ ping -c 1 IP
```

Comprobamos el TTL de la respuesta. Normalmente si es 128 será Windows o 64 Linux

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

## Puertos

Aquí se exponen algunos de los puertos más conocidos y los métodos para obtener información.

### SMB

#### Enum4linux

Enum4linux is a tool for enumerating information from Windows and Samba systems.

```bash
security@beginners:~$ enum4linux [Options] <IP>
```

 ```console
Parámetros de Enum4linux:

 Options are (like "enum"):
    -U        get userlist
    -M        get machine list*
    -S        get sharelist
    -P        get password policy information
    -G        get group and member list
    -d        be detailed, applies to -U and -S
    -u user   specify username to use (default "")
    -p pass   specify password to use (default "")

The following options from enum.exe aren't implemented: -L, -N, -D, -f

Additional options:
    -a        Do all simple enumeration (-U -S -G -P -r -o -n -i).
              This opion is enabled if you don't provide any other options.
    -h        Display this help message and exit
    -r        enumerate users via RID cycling
    -R range  RID ranges to enumerate (default: 500-550,1000-1050, implies -r)
    -K n      Keep searching RIDs until n consective RIDs don't correspond to
              a username.  Impies RID range ends at 999999. Useful
          against DCs.
    -l        Get some (limited) info via LDAP 389/TCP (for DCs only)
    -s file   brute force guessing for share names
    -k user   User(s) that exists on remote system (default: administrator,guest,krbtgt,domain admins,root,bin,none)
              Used to get sid with "lookupsid known_username"
              Use commas to try several users: "-k admin,user1,user2"
    -o        Get OS information
    -i        Get printer information
    -w wrkg   Specify workgroup manually (usually found automatically)
    -n        Do an nmblookup (similar to nbtstat)
    -v        Verbose.  Shows full commands being run (net, rpcclient, etc.)
```

#### Acccheck

The tool is designed as a password dictionary attack tool that targets windows authentication via the SMB protocol. <https://github.com/qashqao/acccheck> for more information.

```bash
security@beginners:~$ ./acccheck.pl -t IP -U <users_dictionary> -P <pass_dictionary> -v
```

#### Smbclient

Shared folder list:

```bash
security@beginners:~$ smbclient -L <IP> -U <user>%<password>
```

Connect:

```bash
security@beginners:~$ smbclient //<ip>/<folder>$ -U <user>%<password>
```

### FTP

### LDAP

```bash
security@beginners:~$ nmap -p 389 --script ldap-search <IP>
```

### Kerberos

```console
security@beginners:~$ nmap -Pn -p 88 --script=krb5-enum-users --script-args krb5-enum-users.realm="dominio" <IP>
```
