# Overview
O objetivo do projeto é permitir o fácil gerenciamento de senhas, segredos, arquivos de chaves ssh e etc.
# Usagem
```
NAME:
   secret - Guarde e manipule senhas e segredos

USAGE:
   Guarde e manitupe segredos no geral: senhas, ssh keys, tokens e etc

COMMANDS:
   new, n   Novo segredo
   get, g   Recupera um segredo
   list, l  Lista todos os segredos disponíveis, ocultando os valores
   key, k   configure a chave de 32bit

GLOBAL OPTIONS:
   --help, -h  show help
```
## Configurando a chave para ocultar os segredos
```bash
secret help key
NAME:
   secret key - configure a chave de 32bit

USAGE:
   Configure a chave de 32bit que será utilizada para realizar a criptografia e a decriptografia

OPTIONS:
   --token value, -t value  chave pessoal
   --generate, -g           gerar chave (default: false)
   --help, -h               show help
```
Para realizar o armazenamento dos segedos, será usado o algoritmo de criptografia simétrica. Esse algoritmo consiste em utilizar uma chave de 32 caracteres, para passar no momento de codificar e decodificar a informação. É uma forma segura e prática para esconder e recuperar os dados escondidos, o unico requisito é que a chave esteja salva em algum lugar.

Caso a chave se perca, não será mais possível decodificar os dados escondidos.

- Gerar uma chave de 32bits
```bash
$ secret key --generate
chave gerada: 424f26a061636972706e2c776e6963686273696e676b6370
```
- Utilize uma chave própria de 32bits
```bash
$ secret key --token euqueroumachavesuperseguraparapp
```

A chave vai ser armazenada internamente, não sendo necessário o usuário informar o mesmo a cada manipulação dos segredos.
## Criando segredo
```bash
secret help new
NAME:
   secret new - Novo segredo

USAGE:
   secret new [command options]

OPTIONS:
   --name value, -n value    nome único para rotular o segredo
   --secret value, -s value  segredo
   --description value, -d value, --desc value  descrição do segredo
   --email value, -e value   email para receferencia
   --url value, -u value     email para referencia
   --file value, -f value    arquivo para ser armazenado
   --help, -h                show help
```
- Criando um novo segredo
```bash
$ secret new --name azure-devops-token --secret 7f582271-6578-4b17-b6e4-4ec7a8fbff0b
```
- Criando um novo segredo referenciando um arquivo
``` bash 
$ secret new --name azure-devops-token --file some_file.pem
```
- Criando um novo segredo referenciando um arquivo e um segredo
```bash
$ secret new --name azure-devops-token --secret 7f582271-6578-4b17-b6e4-4ec7a8fbff0b --file some_file.pem
```
- Criando um novo segredo com todas as informações possíveis
```bash
$ secret new --name azure-devops-token --secret 7f582271-6578-4b17-b6e4-4ec7a8fbff0b --file some_file.pem --email "email@gmail.com" --url "github.com"
```
## Buscando segredo
```bash
secret help get
NAME:
   secret get - Recupera um segredo

USAGE:
   secret get [command options]

OPTIONS:
   --name value, -n value  nome único para rotular o segredo
   --spy, -s               se informado, retornará as informações ocultas do segredo (default: false)
   --path value, -p value  se informado, irá criar o contúdo do segredo no caminho especificado
   --omit value, -o value [ --omit value, -o value ]  omitir colunas
   --help, -h              show help
```

- Buscando um segredo e ocultando os valores sensíveis
```bash
$ secret get --name azure_devops_token --omit email,url,desc
name               createAt            updateAt
azure_devops_token 20-04-2024 22:03:10 20-04-2024 22:03:10
```
- Buscando um segredo e retornando o valor sensível
```bash
$ secret get --spy --name azure_devops_token --omit email,url,desc
name               createAt            updateAt            value
azure_devops_token 20-04-2024 22:03:10 20-04-2024 22:03:10 7f582271-6578-4b17-b6e4-4ec7a8fbff0b
```
- Buscando um segredo, retornando o valor sensível e exportando o arquivo
```bash
$ secret get --spy --name azure_devops_token --path "dir/path/target" --omit email,url,desc
name               createAt            updateAt            value
azure_devops_token 20-04-2024 22:03:10 20-04-2024 22:03:10 7f582271-6578-4b17-b6e4-4ec7a8fbff0b
```
- Buscando um segredo sem omitir nenhuma coluna
```bash
$ secret get --spy --name azure_devops_token
name               description                                  createAt            updateAt            email           url        value              
azure_devops_token azure devops token para consultar as tarefas 20-04-2024 22:03:10 20-04-2024 22:03:10 email@gmail.com google.com 7f582271-6578-4b17-b6e4-4ec7a8fbff0b
```
