# Samba 4 AD container based on Alpine Linux 3.4

## Credits

This repo has been created based on [tkaefer/alpine-samba-ad-container/](https://hub.docker.com/r/tkaefer/alpine-samba-ad-container/)

Some parts are collected from:

* [Osirium/docker-samba-ad-dc](https://github.com/Osirium/docker-samba-ad-dc)
* [myrjola/docker-samba-ad-dc](https://github.com/myrjola/docker-samba-ad-dc)

>You can read more about __Samba, Active Directory & LDAP__ [here](https://wiki.samba.org/index.php/Samba,_Active_Directory_%26_LDAP)

## Usage

Without any config and thrown away when terminated:

```sh
docker run -it --rm militellovinx/samba-ad
```

## Environment variables

Environment variables are controlling the way how this image behaves, therefore please check this list an explanation:

| Variable              | Explanation                                                                                                    | Default                     |
| ---------------------- | -------------------------------------------------------------------------------------------------------------- | --------------------------- |
| `SAMBA_DOMAIN`         | The domain name used for Samba AD                                                                              | `SAMDOM`                    |
| `SAMBA_REALM`          | The realm for authentication (eg. Kerberos)                                                                    | `SAMDOM.EXAMPLE.COM`        |
| `LDAP_ALLOW_INSECURE`  | Allow insecure LDAP setup, by using an unecrypted password. *Please use only in debug and non-productive setups.* | `false`                     |
| `SAMBA_ADMIN_PASSWORD` | The samba admin user password                                                                                  | set to `$(pwgen -cny 10 1)` |
| `KERBEROS_PASSWORD`    | The Kerberos password                                                                                          | set to `$(pwgen -cny 10 1)` |

## Use existing data

Using (or reusing data) is done by providing

* `/etc/samba/smb.conf`
* `/etc/krb5.conf`
* `/usr/lib/samba/`
* `/var/lib/krb5kdc/`

as volumes to the docker container.

## Example

### Plain docker

```sh
touch /tmp/krb-conf/krb5.conf

docker run -d -e SAMBA_DOMAIN=TEST -e SAMBA_REALM=TEST.MYDOMAIN.COM -v /tmp/smb-conf:/etc/samba -v /tmp/krb-conf/krb5.conf:/etc/krb5.conf -v /tmp/smb-data:/var/lib/samba -v /tmp/krb-data:/var/lib/krb5kdc --name smb4ad militellovinx/samba-ad
```

For details on how to store data in directories, containers etc. please check the [Docker documentation](https://docs.docker.com/).
