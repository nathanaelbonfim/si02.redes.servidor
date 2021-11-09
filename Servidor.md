# Alunos
> RA: 00210090  - Gabriel Ciolin Fasolo
> RA: 00210465 - Nathanel Cavalcanti Bonfim
> RA: 00210594 - Lucas de Barros Siqueira
> RA: 00212191 - Vitor Eduardo da Silva Gibim
> RA: 00204966 - Alexandre Gonçalves Silva

# Instalação do servidor SSH
```bash
sudo apt install ssh-server
```

## Configuração do servidor SSH
```bash
vi /etc/ssh/sshd_config
```

```sshconfig
+ Port 5959                  # Altera a porta padrão para 5959
+ PasswordAuthentication no  # Desabilita o login por senha (só com chave SSH)
```

# Instalação do Apache

```bash
apt install apache2
```

# Configuração do firewall
### Bloqueio de todas as conexões por padrão
```bash
sudo ufw default deny incoming
```

### Permitindo o tráfego apenas na porta 80 (apache2) e 5959 (ssh)
```bash
sudo ufw allow in "Apache"
```
# Instalação do MariaDB (banco de dados)

```bash
sudo apt install mariadb-server
```
### Configurando MariaDB

```bash
mysql_secure_installation
```

# Instalação do PhpMyadmin

```bash
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
### Habilitar a extensão PHP mbstring 

```bash
sudo phpenmod mbstring
```
### Reiniciando o Apache

```bash
systemctl restart apache2
```

### Criando um usuário para o phpMyAdmin

```bash
sudo mariadb
```
```bash
CREATE USER 'NOME_DO_USUARIO'@'localhost' IDENTIFIED BY 'SENHA';
```

### Garantindo todos os privilégios para o usuário

```bash
GRANT ALL PRIVILEGES ON *.* TO 'NOME_DO_USUARIO'@'localhost' WITH GRANT OPTION;
```

```bash 
exit;
```


# Instalação do Firebird

### Baixar o firebird
```bash
wget https://github.com/FirebirdSQL/firebird/releases/download/v4.0.0/Firebird-4.0.0.2496-0.amd64.tar.gz
```

### Extração do arquivo
```bash
tar -xvzf Firebird-4.0.0.2496-0.amd64.tar.gz
```
```bash
cd Firebird-4.0.0.2496-0.amd64
```

### Instalação
```bash
./install.sh
```


### Criação do diretório do banco de dados
```bash
mkdir -p /var/lib/firebird
```

### Permissões para o grupo firebird
```bash
chown -R /var/lib/firebird
```
 
### Adição do usuário atual ao grupo firebird
```bash
usermod -a -G $(whoami) firebird
```

### Disponibilização do executável no $PATH
```bash
ln -s /opt/firebird/bin/isql /usr/local/bin
```

### Criação do banco de dados
```bash
isql 
```


### ISQL 
```sql
CREATE DATABASE "/var/lib/firebird/agenda.fdb";
```

### SQL do banco
```sql
--- Tabela de usuários
CREATE TABLE usuario (
	id INTEGER NOT NULL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	email VARCHAR(255) NOT NULL,
	hash_senha VARCHAR(255) NOT NULL
);

--- Tabela de contato
CREATE TABLE contato (
	id INTEGER NOT NULL PRIMARY KEY,
	nome VARCHAR(255),
	sobrenome VARCHAR(255) NOT NULL,
	telefone VARCHAR(80) NOT NULL,
	anotacao VARCHAR(2048)
);

```

# Instalação de antivírus
```bash
sudo apt install clamav clamav-daemon
```

## Varreduras periódicas a cada hora
```bash
crontab -u clamav -e
```

```crontab
# m h  dom mon dow   command

0 */4 * * * clamscan --recursive / >> /var/clamv/log.txt  # Escaneia o servidor a cada 4 horas
* */2 * * * /usr/local/bin/freshclam                      # Atualiza a base de dados de duas em duas horas
```

## Repositório para a base da dados
> /usr/local/etc/freshclam.conf 

```conf
+ DatabaseMirror db.local.clamav.net
+ DatabaseMirror database.clamav.net
+ DatabaseMirror clamavdb.c3sl.ufpr.br
```

# Configuração de rotação de logs
```conf
/var/log/clamav/scan.log {
        rotate 30          # Mantêm 30 arquivos no histórico
        daily              # Rotaciona diariamente

        notifempty         # Notifica o usuário caso
						   # não haja nenhum.
        compress           # Comprime os arquivos não rotacionados
}
```


# Criação de interfaces virtuais de rede

### Habilitar o módulo de interfaces virtuais no kernel
```bash
sudo modprobe dummy
```

### Adição de interfaces virtuais
```bash
sudo ip link add eth0...eth10 type dummy
```
### Arquivo de configuração DHCP
> /etc/dhcp/dhcpd.conf
```conf
default-lease-time 600;                       
max-lease-time 7200;                          
authoritative;                                
                                              
subnet 192.168.1.0 netmask 255.255.255.240 {  
        range 172.59.0.1 172.59.255.254;      
        option routers 172.59.0.13;           
        option domain-name-servers 172.59.0.1;
}                                             
```


#### Bóson Treinamentos
> /etc/networking/interfaces
```conf
## DHCP Server
allow-hotplug eth1
iface eth1 inet static

adress 192.168.10.1
netmask 255.255.255.0
network 192.168.10.0
broadcast 192.168.1.255
```

#### Reuninicar a placa de rede
```bash
ifconfig eth1 down
ifconfig eth1 up
```
ip addr flush eth1 && systemctl restart networking


vim /etc/network/interfaces
vim /etc/dhcp/dhcpd.conf



# Instalação do nginx

```bash
apt install nginx php7.4-fpm
vim /etc/nginx/sites-available/agenda
```