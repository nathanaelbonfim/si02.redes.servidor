# Alunos
### RA: 00210090  - Gabriel Ciolin Fasolo
### RA: 00210465 - Nathanel Cavalcanti Bonfim
### RA: 00210594 - Lucas de Barros Siqueira
### RA: 00212191 - Vitor Eduardo da Silva Gibim
### RA: 00204966 - Alexandre Gonçalves Silva
#
# Instalação do Apache

```bash
apt install apache2
```

### Permitindo o tráfego apenas na porta 80

```bash
sudo ufw allow in "Apache"
```
# Instalação do MariaDB

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
