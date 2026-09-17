# 📘 01 - Comandos Básicos do Git

Vou criar o conteúdo completo dessa pasta. Você pode copiar direto para o arquivo `01-comandos-basicos/README.md`.

## 📄 Conteúdo do README.md

```markdown
# 01 - Comandos Básicos do Git

Nesta seção estão os comandos essenciais para começar a usar o Git no dia a dia.

---

## 🔧 Configuração inicial

Antes de começar, configure seu usuário (feito uma vez por máquina):

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
```

Verificar configurações:

```bash
git config --list
```

Definir branch padrão como `main`:

```bash
git config --global init.defaultBranch main
```

---

## 🆕 git init

Inicializa um repositório Git em uma pasta.

```bash
git init
```

Cria uma pasta oculta `.git/` que guarda todo o histórico do projeto.

---

## 🔍 git status

Mostra o estado atual dos arquivos (modificados, staged, untracked).

```bash
git status
git status -s   # versão resumida
```

**Legenda do `-s`:**
- `??` → arquivo não rastreado (untracked)
- `A` → adicionado (staged)
- `M` → modificado
- `D` → deletado

---

## ➕ git add

Adiciona arquivos à área de **staging** (preparação para o commit).

```bash
git add arquivo.txt          # um arquivo específico
git add pasta/               # uma pasta
git add .                    # tudo que mudou
git add *.js                 # todos os arquivos .js
```

---

## 💾 git commit

Salva as mudanças que estão na área de staging.

```bash
git commit -m "mensagem do commit"
git commit -am "msg"         # add + commit (só p/ arquivos já rastreados)
git commit --amend           # editar o último commit
```

**Boas práticas de mensagem:**
- Use o imperativo: "adiciona", "corrige", "remove"
- Seja curto e claro
- Padrão semântico: `feat:`, `fix:`, `docs:`, `chore:`

Exemplo:
```bash
git commit -m "docs: adiciona anotações sobre git add"
```

---

## 📜 git log

Mostra o histórico de commits.

```bash
git log                          # completo
git log --oneline                # uma linha por commit
git log --oneline --graph        # com gráfico de branches
git log --author="Nome"          # filtrar por autor
git log -p arquivo.txt           # mudanças de um arquivo
```

---

## 🔎 git diff

Mostra as diferenças entre estados dos arquivos.

```bash
git diff                     # mudanças não adicionadas (unstaged)
git diff --staged            # mudanças adicionadas (staged)
git diff HEAD                # tudo que mudou desde o último commit
git diff commit1 commit2     # entre dois commits
```

---

## 🗑️ Remover / mover arquivos

```bash
git rm arquivo.txt           # remove do disco e do Git
git rm --cached arquivo.txt  # remove só do Git (mantém no disco)
git mv antigo.txt novo.txt   # renomeia/move
```

---

## ⏪ Desfazendo coisas

```bash
git restore arquivo.txt              # descarta mudanças não staged
git restore --staged arquivo.txt     # tira do staging (mantém mudanças)
git reset --soft HEAD~1              # desfaz último commit, mantém mudanças staged
git reset --mixed HEAD~1             # desfaz commit, tira do staging (padrão)
git reset --hard HEAD~1              # ⚠️ desfaz TUDO (perde alterações)
```

---

## 🧠 Fluxo básico resumido

```
1. Editar arquivos
2. git status          → ver o que mudou
3. git add .           → preparar mudanças
4. git commit -m "..." → salvar no histórico
5. git log --oneline   → conferir
```

---

## 📌 Resumo rápido

| Comando | Para que serve |
|---------|----------------|
| `git init` | Inicia um repositório |
| `git status` | Mostra o estado dos arquivos |
| `git add` | Prepara arquivos para commit |
| `git commit` | Salva as mudanças no histórico |
| `git log` | Mostra o histórico de commits |
| `git diff` | Mostra as diferenças |
| `git restore` | Descarta mudanças |
| `git rm` | Remove arquivos |

