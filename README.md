Usarei como exemplo o Bitbucket, mas funciona para o Github e Gitlab.

Crie os arquivos ssh para os dois usuários:

```bash
ssh-keygen -f ~/.ssh/id_rsa_user1

ssh-keygen -f ~/.ssh/id_rsa_user2
```

Inicie o ssh-agent:
```bash
eval $(ssh-agent)
```

Crie ou edite o arquivo de configuração do ssh
```bash
sudo vim ~/.ssh/config
```

O arquivo ~/.ssh/config deve estar assim:

Importante: o atributo User se refere ao usuário ao qual você irá incluir a chave pública do SSH.

```bash

Host id_rsa_user1
    HostName bitbucket.org
    User user1
    IdentityFile ~/.ssh/id_rsa_user1

Host id_rsa_user2
    HostName bitbucket.org
    User user2
    IdentityFile ~/.ssh/id_rsa_user2

Host *
    IgnoreUnknown UseKeychain
    UseKeychain yes
    AddKeysToAgent yes
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_user1
    IdentityFile ~/.ssh/id_rsa_user2
    IdentitiesOnly yes
    PreferredAuthentications keyboard-interactive,password
```

Adicione as chaves para o ssh-agent:
```bash
ssh-add ~/.ssh/id_rsa_user1

ssh-add ~/.ssh/id_rsa_user2
```

Copie e inclua as chaves públicas no registro de chaves das respectivas contas do Bitbucket

```bash
cat ~/.ssh/id_rsa_user1.pub

cat ~/.ssh/id_rsa_user1.pub
```

Verifique a conexão com o bitbucket
```bash
ssh -T git@bitbucket.org
```

Antes de clonar os repositórios, altere o usuário e o host da URL.

Por exemplo:

de:
*git@bitbucket.org:company/repository_name.git*

para:
*user1@id_rsa_user1:company/repository_name.git*


Caso já tenha feito o clone do repositório, basta trocar a url

```bash
git remote set-url origin user1@id_rsa_user1:company/repository_name.git
```

Infelizmente este processo precisa ser repetido para cada repositório que for clonar.
