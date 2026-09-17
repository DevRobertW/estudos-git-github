# 02 - Branches

Guia completo sobre branches no Git — do conceito básico às situações reais do dia a dia, incluindo os erros mais comuns e como resolvê-los.

---

## 📑 Índice

1. [O que é uma branch?](#1-o-que-é-uma-branch)
2. [Branch principal](#2-branch-principal)
3. [Criar uma branch](#3-criar-uma-branch)
4. [Listar branches](#4-listar-branches)
5. [Trocar de branch](#5-trocar-de-branch)
6. [Criar e trocar para uma branch](#6-criar-e-trocar-para-uma-branch)
7. [Renomear uma branch](#7-renomear-uma-branch)
8. [Excluir uma branch](#8-excluir-uma-branch)
9. [Merge](#9-merge)
10. [Conflitos](#10-conflitos)
11. [Resolver conflitos](#11-resolver-conflitos)
12. [Branches no GitHub](#12-branches-no-github)
13. [Resumo dos comandos](#-resumo-dos-comandos)
14. [Erros comuns e soluções](#-erros-comuns-e-soluções)
15. [Boas práticas](#-boas-práticas)
16. [Fluxo profissional](#-fluxo-profissional)

---

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

Depois que terminar, você pode juntar a branch com a `main`. Isso é chamado de **merge**.

---

## 2. Branch principal

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

Para ver **apenas o nome** da branch atual:

```bash
git branch --show-current
```

---

## 3. Criar uma branch

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

## 4. Listar branches

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

### Ver branches locais **e** remotas

```bash
git branch -a
```

Saída:

```text
* main
  remotes/origin/main
  remotes/origin/feature-login
```

| Referência | O que é |
|------------|---------|
| `main` | Branch **local** (na sua máquina) |
| `origin/main` | Branch **remota** (espelho do GitHub) |

> ⚠️ **Diferença importante:** `origin/` não é uma branch de trabalho. É só uma referência de leitura que o Git mantém para saber onde o GitHub estava no último `fetch`/`push`.

### Ver branches com último commit

```bash
git branch -v
```

Saída:

```text
* main        eb3f7f2 adiciona estrutura inicial
  feature/x   a1b2c3d feat: nova seção
```

---

## 5. Trocar de branch

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

### Forma clássica

Antes do Git 2.23, o comando era:

```bash
git checkout feature-login
```

Ainda funciona, mas o recomendado hoje é `git switch`.

### Voltar para a branch anterior

```bash
git switch -
```

O `-` é atalho para "a última branch em que eu estava".

---

## 6. Criar e trocar para uma branch

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

### Forma clássica

```bash
git checkout -b feature-login
```

---

## 7. Renomear uma branch

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

### Renomeando uma branch específica

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

### ⚠️ Erro comum: espaço na flag

Muita gente digita:

```bash
git branch - m feature-login   ❌
```

O espaço entre `-` e `m` faz o Git imprimir o **manual de ajuda** em vez de renomear. A flag correta é:

```bash
git branch -m feature-login    ✅
```

**Regra de ouro:** nunca separe uma flag curta do seu caractere.

---

## 8. Excluir uma branch

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

### Deletar forçado (sem merge)

Se a branch tem commits que não estão na `main`:

```bash
git branch -D feature-login
```

O `-D` (maiúsculo) deleta **mesmo sem merge**. ⚠️ Pode perder trabalho.

### Deletar branch remota (no GitHub)

```bash
git push origin --delete feature-login
```

### Limpar referências locais de branches remotas já deletadas

```bash
git fetch --prune
```

Remove da lista local as branches remotas que já não existem no GitHub.

---

## 9. Merge

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

### Estratégias de merge

O Git escolhe automaticamente, mas você pode forçar:

| Estratégia | Quando usar |
|------------|-------------|
| `ort` (padrão atual) | Padrão desde o Git 2.34. Substituiu a `recursive` |
| `recursive` (antiga) | Redirecionada para `ort` em versões novas |
| `ours` | Ignora a outra branch, fica só com a sua |
| `theirs` | Ignora a sua, fica só com a outra |

Uso:

```bash
git merge -s ort feature-login
git merge -s ours feature-login
```

> 💡 **Nota:** A estratégia `recursive` foi **substituída pela `ort`** ("Ostensibly Recursive's Twin") no Git 2.34. Ela é mais rápida e resolve menos conflitos. Se você usar `-s recursive` em versões novas, o Git redireciona silenciosamente para `ort`.

---

## 10. Conflitos

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

## 11. Resolver conflitos

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

### Anatomia dos marcadores

| Marcador | Significado |
|----------|-------------|
| `<<<<<<< HEAD` | Início do que **você** escreveu (branch atual) |
| `=======` | Divisor entre as duas versões |
| `>>>>>>> nome-branch` | Fim do que veio da **outra branch** |

### Ver quais arquivos estão em conflito

```bash
git status
```

Saída:

```text
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   app.py
```

### Abortar um merge em andamento

Se der tudo errado:

```bash
git merge --abort
```

Volta tudo como estava antes do merge. **Salvavidas.** 🛟

### Usar um dos lados automaticamente

```bash
# Ficar só com a versão da sua branch atual
git checkout --ours app.py

# Ficar só com a versão que veio do remoto
git checkout --theirs app.py
```

Depois `git add` + `git commit`.

### Conflito resolvido online (GitHub)

Se o conflito aparecer no Pull Request, o próprio GitHub oferece um editor:

1. Clique em **"Resolve conflicts"**
2. Edite o arquivo no navegador
3. Remova os marcadores
4. Clique em **"Mark as resolved"**
5. **"Commit merge"**

---

## 12. Branches no GitHub

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

### Fluxo de Pull Request

1. Crie a branch: `git switch -c feat/minha-tarefa`
2. Trabalhe e commite
3. Envie a branch: `git push -u origin feat/minha-tarefa`
4. No GitHub, clique em **"Compare & pull request"**
5. Descreva o que mudou
6. Clique em **"Create pull request"**
7. Resolva conflitos se houver
8. **"Merge pull request"** → **"Confirm merge"**
9. **"Delete branch"** (limpa o repositório)

---

## 📋 Resumo dos comandos

| Ação | Comando |
|------|---------|
| Ver branch atual | `git branch --show-current` |
| Listar branches locais | `git branch` |
| Listar branches locais + remotas | `git branch -a` |
| Ver branches com último commit | `git branch -v` |
| Criar branch (sem trocar) | `git branch nome` |
| Trocar de branch | `git switch nome` |
| Trocar de branch (clássico) | `git checkout nome` |
| Voltar para a anterior | `git switch -` |
| Criar e trocar | `git switch -c nome` |
| Criar e trocar (clássico) | `git checkout -b nome` |
| Renomear atual | `git branch -m novo-nome` |
| Renomear específica | `git branch -m antigo novo` |
| Deletar (mergeada) | `git branch -d nome` |
| Deletar (forçado) | `git branch -D nome` |
| Deletar remota | `git push origin --delete nome` |
| Limpar referências remotas | `git fetch --prune` |
| Merge na branch atual | `git merge nome` |
| Abortar merge | `git merge --abort` |
| Guardar mudanças | `git stash` |
| Recuperar mudanças | `git stash pop` |

---

## 🐛 Erros comuns e soluções

### ❌ Erro 1 — `git branch - m branch-teste`

```text
usage: git branch [<options>] [-r | -a] [--merged]...
```

**Causa:** espaço entre `-` e `m`. O Git interpretou como duas flags separadas.

**Solução:**

```bash
git branch -m branch-teste
```

**Regra de ouro:** nunca separe uma flag curta do seu caractere.

---

### ❌ Erro 2 — `Your local changes would be overwritten by checkout`

```text
error: Your local changes to the following files would be overwritten by checkout:
    arquivo.txt
Please commit your changes or stash them before you switch branches.
```

**Causa:** você tem arquivos **modificados e não commitados** na branch atual, e a troca vai sobrescrevê-los.

**Soluções (escolha uma):**

**🅰️ Commitar antes:**

```bash
git add .
git commit -m "wip: salvando antes de trocar de branch"
git switch outra-branch
```

**🅱️ Guardar com `stash`:**

```bash
git stash                      # guarda as mudanças
git switch outra-branch        # troca tranquilo
# ... faz o que precisa ...
git switch volta-pra-branch
git stash pop                  # recupera
```

**🅲 Descartar (⚠️ perde tudo):**

```bash
git restore .
git switch outra-branch
```

---

### ❌ Erro 3 — `fatal: a branch named 'main' already exists`

```text
fatal: a branch named 'main' already exists
```

**Causa:** você tentou criar a `main` com `git checkout -b main`, mas ela já existe.

**Solução:** só trocar para ela:

```bash
git switch main
```

Se você **queria** criar uma branch nova, use outro nome.

---

### ❌ Erro 4 — `merge: nova-brach - not something we can merge`

```text
merge: nova-brach - not something we can merge

Did you mean this?
        origin/nova-brach
```

**Causa:** a branch `nova-brach` **não existe localmente** — só no remoto.

**Solução (escolha uma):**

**🅰️ Fazer merge direto da referência remota:**

```bash
git merge origin/nova-brach
```

**🅱️ Criar a branch local primeiro:**

```bash
git switch -c nova-brach origin/nova-brach
git switch main
git merge nova-brach
```

---

### ❌ Erro 5 — Branches duplicadas ou "fantasmas" no GitHub

**Situação:** você tinha `nova-brach`, renomeou para `branch-teste`, deletou localmente, mas o GitHub ainda mostra as duas.

**Causa:** renomear/deletar **localmente** não altera o remoto.

**Solução:**

```bash
# Deletar as branches remotas que não usa mais
git push origin --delete nova-brach
git push origin --delete branch-teste

# Sincronizar a lista local
git fetch --prune

# Conferir
git branch -a
```

---

### ❌ Erro 6 — Trocar de branch com pasta "sumindo"

**Situação:** você troca de branch e uma pasta que você criou **desaparece**.

**Causa:** pastas vazias não são rastreadas pelo Git. Cada branch tem seu próprio "estado" de arquivos.

**Solução:** adicione um `.gitkeep` dentro da pasta:

```bash
touch minha-pasta/.gitkeep
git add minha-pasta/.gitkeep
git commit -m "chore: adiciona .gitkeep em minha-pasta"
```

---

### ❌ Erro 7 — `! [rejected] main -> main (fetch first)`

```text
! [rejected]        main -> main (fetch first)
error: failed to push some refs to '...'
hint: Updates were rejected because the remote contains work that you do not have locally.
```

**Causa:** o GitHub tem commits que você não tem localmente.

**Solução:**

```bash
git pull origin main
# resolve conflitos se houver
git push origin main
```

**Regra de ouro:** sempre `git pull` **antes** de começar a trabalhar.

---

### ❌ Erro 8 — `fatal: refusing to merge unrelated histories`

**Causa:** dois repositórios sem ancestral comum (ex.: você criou um local com `git init` e outro no GitHub).

**Solução:**

```bash
git pull origin main --allow-unrelated-histories
```

---

## ✅ Boas práticas

- **Nomes descritivos:** use prefixos como `feat/`, `fix/`, `docs/`, `chore/`
- **Uma branch por tarefa:** não misture assuntos
- **Branches curtas:** vida útil de horas ou dias, não semanas
- **Sempre partir da `main` atualizada:**
  ```bash
  git switch main
  git pull origin main
  ```
- **Deletar depois do merge:** mantém o repositório limpo
- **Nunca commitar direto na `main`** em projetos com equipe
- **Sempre `git pull` antes de começar** a trabalhar

---

## 🎯 Fluxo profissional

### Para cada tarefa nova

```bash
# 1. Atualizar a main
git switch main
git pull origin main

# 2. Criar a branch da tarefa
git switch -c feat/minha-tarefa

# 3. Trabalhar e commitar
git add .
git commit -m "feat: descrição clara"

# 4. Enviar a branch para o GitHub
git push -u origin feat/minha-tarefa

# 5. Abrir Pull Request no GitHub

# 6. Depois do merge, limpar
git switch main
git pull origin main
git branch -d feat/minha-tarefa
git push origin --delete feat/minha-tarefa
```

### Exemplo prático completo

```bash
# Começar do zero
git switch main

# Criar funcionalidade
git switch -c dashboard

# Trabalhar
git status
git add .
git commit -m "Adiciona dashboard de viagens"

# Voltar para a main
git switch main

# Fazer merge
git merge dashboard

# Deletar a branch
git branch -d dashboard
```

Fluxo visual:

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

## 🧠 O que memorizar primeiro

Não tente decorar tudo de uma vez.

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

---

## 🎯 Resumo mental

```text
main   →  branch estável, sempre funcionando
  ↑
  │ merge (via PR)
  │
feat/x →  branch de trabalho, vida curta
```

- Crie → trabalhe → envie → PR → merge → delete
- Em caso de dúvida: `git status` primeiro, sempre
- Em caso de pânico: `git merge --abort` ou `git stash`
- Em caso de erro estranho: copie a mensagem inteira e pesquise

---

➡️ [Voltar ao README principal](../README.md)
