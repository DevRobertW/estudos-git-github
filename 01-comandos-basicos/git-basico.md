Claro. Essa tabela contém praticamente o **núcleo do Git local**. Se você dominar esses 8 comandos, já terá uma base muito boa antes de entrar em `branch`, `merge`, `push`, `pull` e GitHub.

Vou explicar **o que acontece por trás de cada comando**, com exemplos práticos.

---

# 🧠 Primeiro: entenda o ciclo do Git

Antes dos comandos, grave este desenho:

```text
                 VOCÊ EDITA
                     │
                     ▼
              ┌─────────────┐
              │   Arquivos  │
              │ modificados │
              └──────┬──────┘
                     │
                  git add
                     │
                     ▼
              ┌─────────────┐
              │   STAGING   │
              │    AREA     │
              └──────┬──────┘
                     │
                git commit
                     │
                     ▼
              ┌─────────────┐
              │  HISTÓRICO  │
              │    GIT      │
              └─────────────┘
```

E:

```text
git status → "O que está acontecendo?"
git diff   → "O que mudou?"
```

---

# 1. `git init`

## Para que serve?

Cria um **repositório Git** dentro da pasta atual.

Imagine que você criou:

```text
meu-projeto/
├── app.py
├── banco.py
└── README.md
```

Entre na pasta:

```bash
cd meu-projeto
```

E execute:

```bash
git init
```

O Git vai criar uma pasta oculta:

```text
meu-projeto/
├── .git/
├── app.py
├── banco.py
└── README.md
```

Essa pasta:

```text
.git
```

é extremamente importante.

Ela contém as informações necessárias para o Git controlar o histórico do projeto.

### Depois do `git init`

Você pode verificar:

```bash
git status
```

Provavelmente verá algo parecido com:

```text
On branch master

No commits yet

Untracked files:
    app.py
    banco.py
    README.md
```

Ou seja:

> "Eu comecei a controlar essa pasta, mas ainda não tenho nenhum commit."

### ⚠️ Cuidado

Não saia executando:

```bash
git init
```

em qualquer pasta.

Se você estiver dentro de:

```text
/home/robert/
```

e fizer:

```bash
git init
```

pode transformar sua pasta pessoal inteira em um repositório.

O ideal é:

```bash
cd ~/meu-projeto
git init
```

---

# 2. `git status`

Esse é um dos comandos **mais importantes do Git**.

```bash
git status
```

Ele mostra a situação atual do seu projeto.

Por exemplo:

```text
On branch main

Changes not staged for commit:

    modified: app.py

Untracked files:

    teste.py
```

Temos duas situações diferentes.

### `modified`

```text
app.py
```

Já era conhecido pelo Git, mas foi alterado.

### `untracked`

```text
teste.py
```

É um arquivo novo que o Git ainda não está acompanhando.

---

## Depois do `git add`

Faça:

```bash
git add app.py
```

Agora:

```bash
git status
```

pode mostrar:

```text
Changes to be committed:

    modified: app.py
```

Isso significa:

```text
app.py
   ↓
modificado
   ↓
git add
   ↓
STAGED
   ↓
pronto para commit
```

---

# 3. `git add`

O `git add` coloca alterações na **Staging Area**.

Exemplo:

```bash
git add app.py
```

Você está dizendo:

> "Git, quero colocar a versão atual desse arquivo no próximo commit."

---

## Adicionar vários arquivos

```bash
git add app.py banco.py
```

---

## Adicionar tudo

```bash
git add .
```

O `.` significa:

> diretório atual e seus arquivos/subdiretórios.

Então:

```bash
git add .
```

é muito usado no dia a dia.

Mas não use cegamente.

Antes:

```bash
git status
```

É uma boa prática.

---

# 4. `git commit`

Agora chegamos a uma das partes mais importantes.

```bash
git commit -m "Adiciona conexão com PostgreSQL"
```

O commit cria um **registro permanente no histórico do Git**.

Imagine:

```text
Commit 1
"Cria estrutura inicial"

Commit 2
"Adiciona conexão com PostgreSQL"

Commit 3
"Corrige consulta de viagens"

Commit 4
"Adiciona filtro por garagem"
```

Cada commit representa um ponto importante da evolução do projeto.

---

## Por que a mensagem é importante?

Evite:

```bash
git commit -m "mudanças"
```

ou:

```bash
git commit -m "teste"
```

Prefira:

```bash
git commit -m "Adiciona filtro por garagem"
```

ou:

```bash
git commit -m "Corrige carregamento das viagens"
```

ou:

```bash
git commit -m "Cria endpoint de consulta de viagens"
```

Assim, quando você olhar o histórico daqui a alguns meses, entenderá o que fez.

---

# 5. `git log`

Mostra o histórico dos commits.

```bash
git log
```

Você verá algo parecido com:

```text
commit 8a31f92
Author: Robert
Date:   ...

    Adiciona filtro por garagem

commit 72ab431
Author: Robert
Date:   ...

    Corrige consulta de viagens

commit 4fd821a
Author: Robert
Date:   ...

    Cria estrutura inicial
```

Cada commit possui um identificador.

Por exemplo:

```text
8a31f92
```

Esse código é o **hash do commit**.

---

## Uma forma mais bonita

```bash
git log --oneline
```

Resultado:

```text
8a31f92 Adiciona filtro por garagem
72ab431 Corrige consulta de viagens
4fd821a Cria estrutura inicial
```

Esse comando é excelente para consultar rapidamente o histórico.

---

# 6. `git diff`

O `git diff` responde:

> **"Exatamente o que eu alterei?"**

Imagine que você tinha:

```python
nome = "Robert"
```

e mudou para:

```python
nome = "Robert William"
```

Execute:

```bash
git diff
```

Você poderá ver:

```diff
- nome = "Robert"
+ nome = "Robert William"
```

O:

```text
-
```

representa algo removido.

O:

```text
+
```

representa algo adicionado.

---

# ⭐ `git diff` é muito importante

Imagine que você passou duas horas programando.

Antes de fazer:

```bash
git add .
git commit
```

faça:

```bash
git diff
```

Você pode descobrir:

> "Opa! Eu alterei esse arquivo sem querer."

Isso evita colocar alterações erradas no commit.

---

## E depois do `git add`?

Existe:

```bash
git diff --staged
```

Ele mostra:

> "O que está atualmente preparado para entrar no próximo commit?"

Então:

```text
git diff
```

→ alterações ainda fora da staging.

```text
git diff --staged
```

→ alterações dentro da staging.

---

# 7. `git restore`

Esse comando é muito interessante porque permite **voltar atrás em alterações**.

Imagine:

```text
app.py
```

Você fez várias alterações, mas percebeu:

> "Não quero essas alterações."

Se elas ainda não foram adicionadas:

```bash
git restore app.py
```

O Git restaura o arquivo para a versão que estava no último commit.

⚠️ **Cuidado:** as alterações descartadas dessa forma podem ser perdidas.

---

## Exemplo

Você tinha:

```python
idade = 30
```

Alterou para:

```python
idade = 31
```

Mas decidiu que não quer a alteração.

```bash
git restore app.py
```

Volta para:

```python
idade = 30
```

---

# `git restore --staged`

Existe outra situação.

Você fez:

```bash
git add app.py
```

Mas percebeu:

> "Não quero que esse arquivo faça parte do próximo commit."

Você pode retirar o arquivo da staging:

```bash
git restore --staged app.py
```

Isso **não apaga suas alterações**.

Essa diferença é fundamental:

```text
git restore arquivo
```

→ descarta a alteração.

Enquanto:

```text
git restore --staged arquivo
```

→ tira o arquivo da staging, mas mantém a alteração.

---

# 8. `git rm`

O `git rm` remove um arquivo **do projeto e do controle do Git**.

Por exemplo:

```bash
git rm teste.py
```

Isso remove:

```text
teste.py
```

do diretório e prepara a remoção para o próximo commit.

Depois:

```bash
git commit -m "Remove arquivo de teste"
```

Agora o histórico registra:

> O arquivo foi removido.

---

# ⚠️ `rm` vs `git rm`

No Linux:

```bash
rm teste.py
```

remove o arquivo do computador.

O Git depois perceberá:

```text
deleted: teste.py
```

Você teria que fazer:

```bash
git add teste.py
```

ou:

```bash
git add .
```

para registrar a remoção.

Com:

```bash
git rm teste.py
```

você já faz a remoção e prepara a alteração para o commit.

---

# 🧩 Agora juntando tudo

Imagine seu projeto:

```text
cittati/
├── app.py
├── banco.py
├── api.py
└── teste.py
```

Você modifica:

```text
app.py
banco.py
```

Primeiro:

```bash
git status
```

Git:

```text
modified: app.py
modified: banco.py
```

Você verifica:

```bash
git diff
```

Analisa as alterações.

Depois:

```bash
git add app.py banco.py
```

Confere:

```bash
git status
```

Agora estão:

```text
Changes to be committed:
    modified: app.py
    modified: banco.py
```

Você pode conferir novamente:

```bash
git diff --staged
```

Tudo certo.

Então:

```bash
git commit -m "Atualiza integração com Cittati"
```

E finalmente:

```bash
git status
```

Resultado:

```text
nothing to commit, working tree clean
```

🎉 Seu projeto está limpo.

---

# 🧠 A tabela que eu recomendo você guardar

| Comando       | Pergunta que ele responde                           |
| ------------- | --------------------------------------------------- |
| `git init`    | "Quero começar a usar Git nesta pasta."             |
| `git status`  | "Como está meu projeto?"                            |
| `git add`     | "Quais alterações quero colocar no próximo commit?" |
| `git commit`  | "Quero registrar essas alterações no histórico."    |
| `git log`     | "O que já foi registrado?"                          |
| `git diff`    | "O que eu alterei?"                                 |
| `git restore` | "Quero desfazer uma alteração."                     |
| `git rm`      | "Quero remover este arquivo do projeto."            |

---

# 🔥 O fluxo que você deve decorar

```bash
git status
```

↓

```bash
git diff
```

↓

```bash
git add .
```

↓

```bash
git diff --staged
```

↓

```bash
git commit -m "Descrição da alteração"
```

↓

```bash
git status
```

E o conceito principal:

```text
          EDITAR
             ↓
       git status
             ↓
        git diff
             ↓
         git add
             ↓
      STAGING AREA
             ↓
        git commit
             ↓
        HISTÓRICO
             ↓
         git log
.
