## Aplicação


### Mysql
CREATE USER 'agenda'@'localhost' IDENTIFIED BY 'Y6EedilKT1Om';

### Senha
CREATE DATABASE agenda;

### Privilegios
GRANT ALL PRIVILEGES ON agenda.* TO 'agenda'@'localhost';

FLUSH PRIVILEGES;O

### sql query
```sql
--- Tabela de usuários
CREATE TABLE usuario (
	id INTEGER NOT NULL PRIMARY KEY AUTO_INCREMENT,
	nome VARCHAR(255) NOT NULL,
	email VARCHAR(255) NOT NULL,
	hash_senha VARCHAR(255) NOT NULL
);

--- Tabela de contato
CREATE TABLE contato (
	id INTEGER NOT NULL PRIMARY KEY AUTO_INCREMENT,
	nome VARCHAR(255),
	sobrenome VARCHAR(255) NOT NULL,
	telefone VARCHAR(80) NOT NULL,
	anotacao VARCHAR(2048)
);

```