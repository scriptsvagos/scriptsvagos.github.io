# Compilar OpenSSH 9.8p1 OracleLinux 7.9

> Cabe aclarar que esto funciona para redhat,rocky,almalinux :D

## **Compilar OpenSSL 1.1.1**
> Para compilar openssh necesitamos por lo menos la versión 1.1.1 de openssl, la version 7.9 de OracleLinux tiene la versión 1.0.2

#### Instalar Dependencias:

    # yum install -y perl-core make gcc wget

#### Descargar openssl con wget:
	wget https://www.openssl.org/source/old/1.1.1/openssl-1.1.1w.tar.gz

#### Descomprimir openssl y acceder a la carpeta:
	tar -xvf openssl-1.1.1w.tar.gz && cd openssl-1.1.1w

#### Ejecutar el siguente comando para configurar y verificar que cumplimos con los requisitos:
> si no estan usando redhat o alguna clónica verificar donde estan las carpetas de ssl, ya que puede ser diferente por distribución

	./config --prefix=/usr --openssldir=/etc/ssl

#### Compilar:
	make 

#### Instalar:
	make install

#### Verificar la versión de Openssl:
	openssl version

### Listo!, ya tenemos OpenSSL la versión 1.1.1, tendria que aparecerles algo asi:
> OpenSSL 1.1.1w  11 Sep 2023

<p>&nbsp;</p>

## **Compilar OpenSSH**

#### Instalar Depedencias: 
    # yum install -y gcc make zlib-devel openssl-devel pam-devel wget

#### Descargar con wget:
	wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.8p1.tar.gz

#### Descomprimir openssh y acceder a la carpeta:
	tar -xzvf openssh-9.8p1.tar.gz && cd openssh-9.8p1

#### Ejecutar el siguente comando para configurar y verificar que cumplimos con los requisitos:
> si no estan usando redhat o alguna clónica verificar donde estan las carpetas y configs de ssh_config, sshd_config, ya que puede ser diferente por distribución

	 ./configure --prefix=/usr --sysconfdir=/etc/ssh --with-pam

#### Compilar:
	make

#### Instalar:
	make install

#### Verificar la versión de OpenSSH:
	ssh -V

### Listo!, ya tenemos OpenSSH la versión 9.8.p1, tendria que aparecerles algo asi:
> OpenSSH_9.8p1, OpenSSL 1.1.1w  11 Sep 2023
