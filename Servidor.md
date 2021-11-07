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
CREATE DATABASE "/var/lib/firebird/agenda.fdb"
```

### SQL do banco
