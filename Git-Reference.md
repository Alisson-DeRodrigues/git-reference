# Git e Github:
Guia de referência de comandos git e configuração de repositório remoto github. Este guia é especialmente útil para quem já possui familiaridade com os tópicos e sabe o que está procurando, mas também pode ser aproveitado como material de estudo para quem está aprendendo.

## Comandos
- `git --version` - versão atual do git instalado

## Comandos de Configuração
### Variáveis de configuração:
- `git config` - variáveis de configuração do git
- `git config --global --list` - exibe todas as configurações globais
- `git config --list` - exibe todas as configurações de todos os modos

### Usuário e email:
- `git config --global user.name "seu nome"` - configura o nome de usuário para todosos repositórios
- `git config --global user.email seuemail@email.com` - configura o nome de ususário para todos os repositórios
- `git config user.name` - exibe o nome de ususário configurado
- `git config user.mail` - exibe o email configurado

### Branch padrão:
- `git config init.defaultBranch` - exibe o nome da branch padrão configurado
- `git config --global init.defaultBranch main` - altera o nome da branch padrão para main em todos os novos repositórios

### Credencias de acesso remoto:
No github, utiliza-se Token ou SSH.

#### Token:
- `git config --global credential.helper cache` - salva uma credencial temporariamente; omitir o global salva apenas para um repositório específico, válido para a próxima credencial que for utilizada
- `git config -- global credential.helper store` - salva uma credencial permanentemente, omitir o global salva apenas para um repositório específico; válido para a próxima credencial que for utilizada
- `git config --global credential.helper` - exibe onde as credenciais estão sendo salvas (cache ou store)
- `git config --global --show-origin credential.helper` - exibe o local onde está armazenado a variável de configuração credential.helper e o valor, o local do arquivo .gitconfig

#### SSH:
**Exibir chaves SSH na máquina:**
- `ls -al ~/.shh` - exibe as chaves SSH na máquina se houver

**Gerar chaves SSH:**
- `ssh-keygen -t ed25519 -C "your_email_github@ex.com"`
    - gera uma chave pública e privada para uso com o SSH
    - `-t ed25519` - tipo de algoritmo de criptografia
    - `-C "your_email_github@ex.com"` - email do repositório github
    - também pedirá para criar uma passphrase

**Adicionar chaves ao ssh-agent:**
- `eval "$(ssh-agent -s)"` - inicia o ssh-agent
- `ssh-add ~/.ssh/id_ed25519`
    - adiciona a chave ssh que está no caminho especificado
    - pedirá a passphrase criada para essa chave

**Adicionar chave pública SSH ao github:**
- Vá na guia SSH and GPG keys

## Comandos Básicos de Repositório
### Definir o repositório git:
#### Repositório local:
- `git init` - transforma o diretório atual em um repositório git ou inicia o repositório se ele já for um

#### Repositório remoto:
- `git clone linkgithub.git <repo-name>`
    - clona um repositório remoto
    - será solicitado credenciais de acesso se for um repositório privado
    - as credenciais podem ser Token para HTTP e chave pública para SSH
    - `<repo-mame>` - é opcional e define o nome da pasta do repositório remoto clonado localmente em vez de utilizar o nome original
- `git clone linkgithub.git --branch feature-1 --single-branch`
    - clona um branch específico (feature) do repositório remoto
    - `feature-1` - é o nome da branch de destino

#### Definir repositório remoto para push e pull:
- `git remote add origin linkgithub.git`
    - `origin` é o nome do repositório remoto, esse é o nome comumente utilizado

### Repositório local:
#### Informações do repositório:
- `git status` - exibe informações como a branch atual e o estado das alterações do repositório como arquivos que precisam ser preparados para commit e arquivos aguardando commit

O git não reconhece pastas vazias, então `.gitkeep` é uma convenção, um arquivo vazio dentro da pasta vazia, utilizado para o git "reconhecer" pastas vazias.

- `git log` - exibe os commits realizados no repositório local, o histórico de commits; mostra sua data, dono, mensagem, hash de identificação
- `git log --oneline` - exibe o log em uma linha para melhor leitura, um resumo
- `git log -1` - exibe o último commit
- `git log -5 --oneline` - exibe os últimos 5 commits resumidos
- `git reflog` - exibe um log completo com ações realizadas pelo usuário como commit, edição de commits, resets realizados e outros

- `git branch -v` - exibe o último commit de cada branch do repositório

- `git show` - exibe as informações detalhadas do último commit, incluindo se for uma reversão de commit
- `git show HEAD~1` - exibe a informação detalhadas do commit original, não inclui o commit de reverção

#### Preparar arquivo para commit:
- `git add arquivo` - prepara um arquivo específico alterado para commit
- `git add *` - prepara todos os arquivos alterados para commit

#### Desfazer git add (retirar arquivos da área de preparação):
- `git reset nome-do-arquivo` - remove um arquivo da área de preparação, mas isso também deleta as alterações do arquivo
- `git restore --staged nome-do-arquivo` - também remove um arquivo da área de preparação, mas não deleta as alterações

#### Fazer commit:
- `git commit -m "Initial commit"`
    - realiza o commit das alterações
    - grava as alterações atuais no repositório local

#### Diferenças entre o commit e as alterações atuais:
- `git diff` - exibe as alterações dos arquivos em comparação com o último commit

#### Desfazer alterações de um arquivo:
- `git restore arquivo.txt` - restaura um arquivo para o estado do último commit descartando as alterações atuais

#### Desfazer commit:
- `git reset --soft hash_do_commit` - volta para um commit, deleta todos os commits posteriores ao commit escolhido e já adiciona os arquivos atuais na área de preparo para serem commitados
- `git reset --mixed hash_do_commit` - é o comportamento padrão do `git reset`, então pode ser omitido; deleta todos os commits posteriores ao commit escolhido e as alterações ficam pendentes para serem adicionadas na área de preparo com `git add`
- `git reset --hard hash_do_commit` - volta para um commit, deleta todos os commits posteriores ao commit escolhido e deleta as alterações dos arquivos
- `git reset HEAD~1` - volta para um commit antes do HEAD (commit atual)

- `git revert HEAD` - reverte um commit sem eliminar seu registro no histórico, além de reverter as alterações do código atual e adicionar no histórico uma mensagem de reversão

#### Alterar mensagem do último commit:
- `git commit --amend -m "Nova mensagem"` - altera a mensagem do último commit no `git log`
- `git commit --amend` - abre o último commit em um editor de texto de terminal como nano ou vim conforme a sua configuração do ambiente

#### Enviar repositório local para remoto:
- `git push -u origin main` - configura a branch `main` como branch de upload de alterações para o repositório remoto `origin` e realiza o primeito envio para o repositório remoto; não esqueça de configurar o repositório remoto com nome origin
- `git push` - empurra as alterações do repositório local para o repositório remoto

#### Recuperar alteraçõe do repositório remoto para o local:
- `git pull` - puxa as alterações de um repositório remoto para o repositório local
    - ele é o equivalente de `git fetch` + `git merge`

#### Recuperar alteraçõe do repositório remoto para o local sem merge:
- `git fetch origin main` - faz o download do repositório remoto para o branch main, seu identificador será origin/main; isso não mescla o repositório remoto no main, apenas cria uma branch com o download

## Trabalhando com branchs
### Criar uma branch:
- `git checkout -b minha-branch` - cria uma branch chamada `minha-branch` e entra nela logo em seguida

Uma branch sempre aponta para o último commit, uma branch ramificada mantém o mesmo histórico de commits do branch main e aponta para o mesmo commit. Entretanto, elas passam a  apontar para um commit diferente quando uma delas realizam um commit.

A boa prática recomenda que os nomes dos branchs começam com nomes como feat/ ou fix/, usa-se barra e não pode ter espaços. Exemplo: `git branch feat/add-create-table-reference`.

### Exibir branchs:
- `git branch` - exibe as branchs presentes no repositório
- `git branch -a` - exibe os branchs incluindo o main

### Navegar entre branchs:
- `git checkout nome-branch` - sai da branch atual e entra na branch especificada

### Renomear branch:
- `git branch -m branch_nome branch_novo_nome` - renomeia um branch

### Deletar uma branch
- `git branch -d minha-branch` -  deleta a branch chamada `minha branch`

### Exibir diferenças entre branchs:
- `git diff main origin/main` - exibe a diferença entre a branch main e a branch origin/main (download do repositório remoto)

## Mesclando branchs com merge e rabase
### Mesclar alterações de branch:
#### Merge básico:
- `git merge minha-branch` - mescla a branch `minha-branch` na branch atual

O merge combina os históricos de commits dos dois branchs, mantendo os históricos em paralelo, depois ele cria um novo commit que une os dois históricos de commits.
```
Antes do merge:

A---B---C (main)
     \
      D---E (feature)

Depois do merge:

A---B---C---------F (main)
     \         /
      D---E---/ (feature)

Perceba que os dois históricos foram mantidos em paralelo e um commit de merge foi criado.
```

**Vantagens do merge:**
- é o mais utilizado em trabalhos colaborativos
- ele mantém o histórico completo, útil para auditoria

**Desvantagens do merge:**
- pode gerar um histórico complexo e difícil de ler

#### Rebase básico:
- `git rebase main` - faz rebase de uma branch para outro, movendo os commits de um para outro

O rebase replica os commits de uma branch em outro branch reescrevendo o histórico de commits parecendo que os commits foram realizados no próprio branch, isso cria um histório que é linear e que não possui um commit de merge.

```
Antes do rebase:

A---B---C (main)
     \
      D---E (feature)

Depois do rebase:

A---B---C---D'---E' (feature)
Perceba que os históricos foram unificados de forma linear e sem um commit de merge
```

**Vantagens do rebase:**
- produz um histórico mais limpo e linear
- facilita leitura e entendimento do histórico
- permite editar o histórico de commit

**Desvantagens do rebase:**
- reescreve o histórico (um problema se a branch for compartilhada).
- pode gerar conflitos que precisam ser resolvidos manualmente.

### Tratamemto de conflitos:
Um conflito ocorre quando se tenta mesclar arquivos de branchs diferentes na branch principal ou em otro branch (remoto ou local), mas o git identifica que há uma versão diferente do mesmo arquivo. Quando isso ocorre, o arquivo do repositório de origem estará mesclado com as duas versões do mesmo arquivo para que manualmente seja determinado qual versão deve ser descartada.
O git nota a diferença primeiro analisando o histórico de commits de ambos os repositórios a partir de um commint comum aos dois repositórios e depois analisando a diferença entre os arquivos; note que o conflito ocorre apenas para uma mesma linha modificada de forma diferente, não há conflito em novas linhas adicionadas.

#### Tratar conflito merge:
**Caso exemplificado:**
- você tenta fazer um merge de uma branch feature para a branch main:
```bash
git checkout main
git merge feature
```
- a seguinte mensagem de conflito aparace:
```pgsql
Auto-merging arquivo.txt
CONFLICT (content): Merge conflict in arquivo.txt
Automatic merge failed; fix conflicts and then commit the result.
```
- o git identifica um conflito, então ele mescla as duas versões do arquivo para vocÊ corrigir manualmente:
```diff
<<<<<<< HEAD
conteúdo da branch atual (main)
=======
conteúdo da branch que está sendo mesclada (feature)
>>>>>>> feature
```
- você corrige o merge apagando a versão indesejada ou mesclando as duas versões manualmente
- depois da correção, você prepara o arquivo e realiza o commit do merge:
```bash
git add arquivo.txt
git commit
```
Esse é o mesmo procedimento para outros comandos que realizam merge além do próprio `git merge` como `git pull` e `git push`. O github possui uma interface (web editor) para tratar push recebidos que resultam em um conflito de merge, mas só para o caso do conflito não ser muito complexo, pois nesse caso utiliza-se a linha de comando.

#### Tratar conflito rebase:
**Caso exemplificado:**
- você tenta fazer um rebase da branch main para uma branch feature:
```bash
git checkout feature
git rebase main
```
- a seguinte mensagem de conflito aparece:
```pgsql
Applying: Seu commit
CONFLICT (content): Merge conflict in arquivo.txt
```
- você corrige o conflito da mesma forma que se faz com o merge
- depois da correção, você prepara o arquivo e continua com o rebase:
```bash
git add arquivo.txt
git rebase --continue
```

### Outros comando do rebase:
#### Cancelar rebase:
- `git rebase --abort` - cancela o rebase em andamento

#### Editar o histórico de commits:
O `git rebase` com `--interactive` ou `-i` permite realizar tarefas com o histórico de commits como editar, apagar, reordenar ou combinar commits de forma interativa, ou seja, pelo seu editor de texto padrão configurado, por exemplo, o nano e o vim.

Tome cuidado, pois editar o histórico de commit com rebase altera o hash dos commits, como se fosse um commit totalmente novo.

- `git rebase --interactive HEAD~2` - abre a interface interativa para editar os dois últimos commits antes do commit atual `HEAD`.

Na interface interativa há palavras-chaveque indicam a ação que você deseja fazer, elas aparecem na interface, mas algumas delas são:
- `d` - drop, remove commit
- `s` - squash, junta commites para melhor organização",
- `r` - reword, altera a mensagem de um commit"

**Exemplo de uma interface:**
```mathematica
Histórico de commit:
A - B - C - D (HEAD)

Interface interativa:
pick C mensagem do commit C
pick D mensagem do commit D

Alteração feita:
reword C nova mensagem para C
squash D com C
```
- `git rebase --interactive --root` - abre a interface interativa de commits, mas abre todo o histórico para edição desde o commit inicial.

## Trabalhando com arquivamento de alterações:
### Arquivar uma alteração:
Consiste em salvar as alterações do repositório em uma lista de alterações chamada `stash` para uso futuro.
- `git stash` - salva as alterações do branch atual e remove as alterações atuais ao mesmo tempo

### Informações da lista de alterações:
- `git stash list` - exibe a lista de alterações
- `git stash show` - fornece informações resumidas sobre as alterações salvas na lista, a última feita
- `git stash show name` - fornece informações resumidas sobre uma alteração específica salva na lista
- `git stash show -p` - fornece informações detalhadas sobre as alterações salvas no último salvamento, inclusive as linhas alteradas
- `git stash show -p name` - fornece informações detalhada sobre um salvamento específico da lista

### Recuperar alteração arquivada:
- `git stash pop` - recupera a última alteração salva na lista e exclui a alteração salva na lista
- `git stash apply` - recupera a última alteração salva na lista, mas não exclui a alteração da lista

### Deletar alteração salva:
- `git stash drop` - deleta a última alteração salva na lista
- `git stash drop <stash_name>` - delata uma alteração específica salva na lista