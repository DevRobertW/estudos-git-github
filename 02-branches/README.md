
# 🌿 Git Branches

## 1. O que é uma branch?

Uma **branch (ramificação)** é uma linha de desenvolvimento do seu projeto.

Imagine que você tem seu projeto:

```text
PROJETO
   │
   ├── commit 1
   ├── commit 2
   └── commit 3
```

Até aqui você está trabalhando normalmente.

Agora você quer desenvolver uma nova funcionalidade, mas **não quer mexer diretamente na versão principal**.

Você cria uma branch:

```text
                 ┌── commit 4
                 │
main ──●──●──●───┤
                 │
                 └── feature-login
```

Na prática:

```text
main
 │
 ●
 │
 ●
 │
 ●
 └──────── feature-login
```

A `feature-login` pode receber vários commits sem alterar diretamente a `main`.

### 🧠 Pense assim:

```text
main
↓
versão principal/estável

feature-login
↓
lugar para desenvolver o login
```

Depois que terminar, você pode juntar a branch com a `main`.

Isso é chamado de **merge**.

---

# 2. Branch principal

A branch principal é normalmente chamada:

```text
main
```

Antigamente era muito comum encontrar:

```text
master
```

Hoje `main` é o nome mais comum.

Você pode verificar em qual branch está:

```bash
git branch
```

Exemplo:

```text
* main
  feature-login
```

O `*` indica:

> Você está atualmente na branch `main`.

---

# 3. Criar uma branch

Para criar uma branch:

```bash
git branch nome-da-branch
```

Por exemplo:

```bash
git branch feature-login
```

Agora:

```text
main
feature-login
```

Mas atenção:

**criar a branch não significa trocar para ela.**

Você ainda está na `main`.

Pode verificar:

```bash
git branch
```

Resultado:

```text
* main
  feature-login
```

---

# 4. Listar branches

Para visualizar suas branches:

```bash
git branch
```

Exemplo:

```text
* main
  feature-login
  correcao-banco
  dashboard
```

O `*` mostra onde você está.

Por exemplo:

```text
  main
* dashboard
  feature-login
```

Significa:

> Estou trabalhando na `dashboard`.

---

# 5. Trocar de branch

Para trocar para uma branch existente:

```bash
git switch nome-da-branch
```

Por exemplo:

```bash
git switch feature-login
```

Agora:

```bash
git branch
```

poderá mostrar:

```text
  main
* feature-login
```

Você saiu da `main` e está trabalhando na `feature-login`.

---

# 6. Criar e trocar para uma branch

Essa é uma combinação muito utilizada.

Em vez de:

```bash
git branch feature-login
git switch feature-login
```

você pode fazer:

```bash
git switch -c feature-login
```

O `-c` significa:

> create — criar.

Então:

```bash
git switch -c feature-login
```

faz duas coisas:

```text
1. Cria a branch
2. Troca para ela
```

Exatamente por isso esse comando é muito usado.

---

# 7. Renomear uma branch

Imagine que você criou:

```bash
git branch login
```

Mas percebeu que o nome poderia ser:

```text
feature-login
```

Você pode renomear:

```bash
git branch -m feature-login
```

O `-m` significa **move/rename** nesse contexto.

Se você estiver atualmente na branch:

```text
login
```

pode fazer:

```bash
git branch -m feature-login
```

Agora:

```text
feature-login
```

---

## Renomeando uma branch específica

Também pode fazer:

```bash
git branch -m login feature-login
```

Isso significa:

```text
login
 ↓
feature-login
```

---

# 8. Excluir uma branch

Imagine que você terminou:

```text
feature-login
```

e já fez o merge dela na `main`.

Agora ela não é mais necessária.

Você pode excluir:

```bash
git branch -d feature-login
```

O `-d` significa delete.

### ⚠️ Importante

Você não pode estar dentro da branch que quer excluir.

Por exemplo:

```text
* feature-login
  main
```

Não faça:

```bash
git branch -d feature-login
```

Primeiro:

```bash
git switch main
```

Depois:

```bash
git branch -d feature-login
```

---

# 9. Merge

Agora chegamos a uma das partes mais importantes.

Imagine:

```text
main
 │
 ●
 │
 ●
 │
 ●
 └──── feature-login
          │
          ●
          │
          ●
```

Você desenvolveu o login na branch:

```text
feature-login
```

Agora quer colocar essas alterações na `main`.

Primeiro vá para `main`:

```bash
git switch main
```

Depois:

```bash
git merge feature-login
```

O Git tenta juntar as alterações.

Ficamos com:

```text
main
 │
 ●
 │
 ●
 │
 ●
 │
 ●──●
     ↑
 alterações da feature-login
```

### 🧠 Regra importante

Você faz o merge **na branch que receberá as alterações**.

Se quer:

```text
feature-login → main
```

faça:

```bash
git switch main
git merge feature-login
```

Não:

```bash
git switch feature-login
git merge main
```

---

# 10. Conflitos

Agora vem uma situação muito comum.

Imagine que você e outra pessoa alteraram a mesma parte do arquivo.

A `main` tem:

```python
nome = "Robert"
```

Sua branch também alterou a mesma linha:

```python
nome = "Robert William"
```

Enquanto outra alteração na `main` colocou:

```python
nome = "Robert Silva"
```

Quando o Git tentar juntar as duas versões, ele pode não saber qual escolher.

Isso é um **conflito**.

Você pode receber algo como:

```text
CONFLICT (content): Merge conflict in app.py
Automatic merge failed
```

O Git está dizendo:

> "Não consigo decidir sozinho qual alteração deve permanecer."

---

# 11. Resolver conflitos

Quando ocorre um conflito, o arquivo pode ficar assim:

```python
<<<<<<< HEAD
nome = "Robert Silva"
=======
nome = "Robert William"
>>>>>>> feature-login
```

Essas marcações significam:

```text
<<<<<<< HEAD
versão atual
```

e:

```text
=======
```

separa as duas versões.

Depois:

```text
>>>>>>> feature-login
```

indica a alteração que veio da outra branch.

Você precisa **editar manualmente o arquivo** e decidir o que deve permanecer.

Por exemplo:

```python
nome = "Robert William Silva"
```

Depois de resolver:

```bash
git add app.py
```

E então:

```bash
git commit
```

O conflito está resolvido.

---

# 12. Branches no GitHub

Até agora estamos falando principalmente do **Git no seu computador**.

O GitHub permite colocar esse repositório na internet e trabalhar com outras pessoas.

Você pode ter:

```text
SEU COMPUTADOR
      │
      │ git push
      ▼
    GitHub
      │
      │ git pull
      ▼
OUTRO COMPUTADOR
```

No GitHub você também terá branches:

```text
main
feature-login
dashboard
correcao-bug
```

Você pode desenvolver:

```text
feature-login
```

e enviar para o GitHub:

```bash
git push origin feature-login
```

Depois pode abrir um **Pull Request** no GitHub para propor:

```text
feature-login
       ↓
      main
```

Isso é muito utilizado em equipes de desenvolvimento.

---

# 🧩 Agora vamos entender todos os comandos

## `git branch`

Lista as branches:

```bash
git branch
```

---

## `git branch nome`

Cria uma branch:

```bash
git branch dashboard
```

Mas **não troca** para ela.

---

## `git switch nome`

Troca para uma branch existente:

```bash
git switch dashboard
```

---

## `git switch -c nome`

Cria e troca para a branch:

```bash
git switch -c dashboard
```

É provavelmente o comando que você mais usará para começar uma nova funcionalidade.

---

## `git branch -m novo-nome`

Renomeia:

```bash
git branch -m dashboard-relatorios
```

---

## `git branch -d nome`

Exclui:

```bash
git branch -d dashboard
```

---

## `git merge nome`

Junta outra branch à branch atual:

```bash
git merge dashboard
```

Lembre:

```text
Estou na main
       ↓
git merge dashboard
       ↓
dashboard → main
```

---

# 🚀 Exemplo completo

Vamos imaginar que você está desenvolvendo seu projeto Cittati.

Você começa:

```bash
git switch main
```

Cria uma funcionalidade:

```bash
git switch -c dashboard
```

Agora você está:

```text
main
 │
 ●
 │
 ●
 └──── dashboard
```

Você programa o dashboard.

Depois:

```bash
git status
```

```bash
git add .
```

```bash
git commit -m "Adiciona dashboard de viagens"
```

Agora volta para:

```bash
git switch main
```

E junta:

```bash
git merge dashboard
```

Depois pode excluir a branch:

```bash
git branch -d dashboard
```

Seu fluxo foi:

```text
                 dashboard
                    │
                    ●
                    │
main ──●────●───────●
                    │
                 desenvolvimento
                    ↓
                  merge
                    ↓
main ──●────●───────●
```

---

# ⭐ O que eu quero que você memorize primeiro

Não tente decorar os 12 tópicos de uma vez.

Comece por estes **4 comandos**:

```bash
git branch
```

**Ver branches**

```bash
git switch main
```

**Trocar de branch**

```bash
git switch -c nova-feature
```

**Criar e entrar em uma branch**

```bash
git merge nova-feature
```

**Juntar a branch com a atual**

E grave esta regra:

> **Para fazer `feature → main`, primeiro entre na `main` e depois faça `git merge feature`.**

```bash
git switch main
git merge feature
```
