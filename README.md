## Simple file server with Samba on Ubuntu
> tutorial in portuguese

Bem, minha intenção com esse repositório é exemplificar e ajudar a todos na criação de um servidor de arquivos feito em Ubuntu e com a documentação em português.

Para instalar o Samba, utilize os seguintes comandos:

```bash
sudo apt update
sudo apt install samba
```

- sudo apt update é usado para atualizar os pacotes da sua máquina;
- sudo apt install samba é usado para instalar o pacote do Samba.

Com o pacote instalado, podemos verificar seus caminhos com o seguinte comando:

```bash
ls /etc/samba
```

Vemos o seguinte arquivo **smb.conf**, que vai ser utilizado para a configuração básica de nosso projeto.

## Configuração inicial do arquivo
> no arquivo smb.conf

Começamos pela configuração global no arquivo:

```bash
[global]
 workgroup = GROUPEXAMPLE
 netbios name = example
```

- workgroup é referente ao grupo de trabalho a ser usado;
- netbios name é o nome do seu domínio.

Com as configurações globais feitas, podemos nos atentar às configurações de nossos primeiros diretórios compartilhados. Para fazermos um teste inicial, iremos utilizar o seguinte serviço:

```bash
[sambashare]
 comment = Samba share on Ubuntu

 path = /your-path/sambashare
 
 read only = no
 writable = yes
 browsable = yes
```

- o nome do nosso serviços está presente dentro dos colchetes: **[sambashare]**;
- comment é simplesmente o comentário inicial, para nos exemplificar o que estamos fazendo dentro do diretório compartilhado;
- path é o caminho até a pasta compartilhada que desejamos.

Agora, vamos nos atentar às opções mais básicas para a configuração de nosso diretório compartilhado:

- **read only**: o diretório compartilhado é apenas para leitura?
- **public**: o diretório é público?

- **writable**: no diretório compartilhado é permitida a escrita?
- **browsable**: no diretório compartilhado é permitida a navegação?

-	**create mode**: formato em octal
-	**directory mode**: formato em octal

- **write list**: lista de usuários ou grupos que é permitido a escrita dentro do diretório

- **recycle:repository**: local da lixeira
- **recycle:keeptree**: manter o local

- **admin users**: usuários administradores do diretório compartilhado

## Opções adicionais

Sincronização com sysvol:

```path
[sysvol]
 path = caminho-até-seu-sysvol
 read only = No
```

Script personalizado de login:

```path
[netlogon]
 path = caminho-até-seu-script-netlogon
 read only = No
```

## Adicionando um novo usuário no nosso servidor

Primeiramente, vamos adicionar o usuário Unix da seguinte maneira:

```bash
sudo adduser your-user
```

Agora, vamos adicionar esse mesmo usuário no Samba da seguinte forma:

```bash
sudo smbpasswd -a your-user
```

## Como checar o servidor através de sua máquina
> smbclient

Iremos utilizar o smbclient para checar nosso primeiro diretório compartilhado. Primeiramente, vamos instalar o pacote:

```bash
sudo apt install smbclient
```

O Smbclient é responsável pela criação de um client para acessarmos nosso serviço e, consequentemente, nosso diretório compartilhado. Para listarmos nossos diretórios compartilhados, iremos utilizar o seguinte comando:

```bash
smbclient -L your-user
```

Após esse comando, a saída será a lista com nossos diretórios compartilhados, seus tipos e o comentário que colocamos primeiramente nas suas configurações. Agora, para acessarmos esses diretórios, iremos utilizar o seguinte comando:

```bash
/usr/bin/smbclient \\\\your-user\\sambashare your-password
```

Dessa forma, iremos acessar a nossa pasta e já podemos, inclusive, conferir os arquivos e as outras pastas presentes nela.

## Conclusão

Terminamos a criação e a configuração de um servidor simples, mas de muita utilidade no dia a dia de um desenvolvedor ou técnico.
