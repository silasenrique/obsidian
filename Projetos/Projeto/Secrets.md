# Overview
O objetivo do projeto é permitir o fácil gerenciamento de senhas, segredos, arquivos de chaves ssh e etc.
# Funcionalidades
- É possível vincular uma fonte de dados customizada para armazenar os segredos
- Configurar a frequência que a senha e o MFA devem ser solicitados
- Todos os segredos serão compostos por um nome, uma senha, um arquivo chave e uma descrição
- Os valores dos segredos só poderão ser consultados individualmente
	- Caso a frequência de senha/MFA não for configurada, a senha será solicitada toda vez que um segredo for consultado
- É possível listar todos os segredos
- O arquivo da chave pode ser exportado para um diretório especificado
# FAQ
### Fonte de dados
A fonte de dados deve ser provida pelo próprio usuário. Ela pode ser uma fonte de dados local ou hospedado de forma externa. É necessário apenas informar a `connection string` e o próprio `scret` configurará toda a infraestrutura necessária.
### Porque o aplicativo não possui uma própria fonte de dados?
Permitir utilização de fontes de dados externas, permite que o usuário compartilhe segredos com outros computadores também.  

Nativamente a ferramenta utilizará o `sqlite3` de forma local.
### Toda vez que listar os segredos, será possível ver o conteúdo deles?
Não! A ferramenta permite sim listar todos os segredos cadastrados, mas as únicas informações que estarão disponíveis nessa lista serão as seguintes: nome do segredo, descrição, data de criação e data de alteração.
### Como faço para ver o conteúdo do segredo?
É necessário realizar uma consulta individual, utilizando o nome único do segredo. Ao realizar essa consulta, será necessário passar a flag `--spy`, que diz para retornar os dados ocultos também.

Quando a flag `--spy` for informada, será solicitado a senha para realizar a consulta.
### Como funciona a senha para utilizar a ferramenta?
Na primeira vez utilizando a ferramenta, será solicitado a confirmação para sempre solicitar uma senha para ver o conteúdo oculto dos segredos. O usuário poderá negar essa etapa e nenhuma senha será solicitada.

A senha não é obrigatória! Ela é apenas uma camada extra de segurança.
### MFA para substituir a senha
Para manter o mínimo de segurança para os usuários que não querem configurar uma senha, o MFA pode ser utilizado sozinho. Toda vez que a flag `--spy` for utilizada, se o MFA estiver configurado, será solicitado o código de acesso.
### Utilizando o MFA e a senha de acesso
É possível realizar a configuração dos dois tipos de autenticação. Primeiro será solicitado a senha e em seguida o MFA.

## Usagem