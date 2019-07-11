# docker 中部署 LDAP (OpenLDAP + phpLDAPadmin)

LDAP 服务器 (OpenLDAP) + LDAP 应用 (phpLDAPadmin)

[TOC]

## 参考资料

[LDAP入门](https://www.jianshu.com/p/7e4d99f6baaf) - 这篇文章介绍了什么是 LDAP ，以及 LDAP 数据存储的基本逻辑

## 准备网络

```sh
docker network create ldap-network
```

## OpenLDAP

[github/osixia/docker-openldap](https://github.com/osixia/docker-openldap) docker 仓库

```sh
docker run --name ldap-server -d \
	-h ldap-server \
	-p 389:389 \
	-p 636:636 \
	-v ldap-db:/var/lib/ldap \
	-v ldap-conf:/etc/ldap/slapd.d \
	--network ldap-network \
	osixia/openldap:1.2.4
```

## phpLDAPadmin

[github/osixia/docker-phpLDAPadmin](https://github.com/osixia/docker-phpLDAPadmin) ldap-web-client

```sh
docker run --name ldap-web-client -d \
	-h lldap-web-client \
	-p 6443:443 \
    --env PHPLDAPADMIN_LDAP_HOSTS=ldap-server \
    --network ldap-network \
    osixia/phpldapadmin:0.8.0
```

## 扩展资料

[docker 搭建 OpenLDAP](https://www.jianshu.com/p/7f0ab0845066)

[LDAP Browser For Linux](http://www.ldapbrowserlinux.com/) linux 下的 ldap 客户端