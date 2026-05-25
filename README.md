# 🎮 INKA - Tradutor de Ren'Py

Tradutor automático de jogos Ren'Py / Visual Novels com suporte a **8 provedores de tradução** (incluindo Google grátis), proteção inteligente de variáveis, memória de tradução persistente e detecção automática de idioma.

[![Versão](https://img.shields.io/badge/versão-0.1.1-orange)](https://github.com/shedins/inkatranslate/releases)
[![Status](https://img.shields.io/badge/status-beta-yellow)]()
[![Licença](https://img.shields.io/badge/licença-MIT-green)](LICENSE)
[![Plataforma](https://img.shields.io/badge/plataforma-Windows-lightgrey)]()

---

## ✨ Principais recursos

- 🌐 **8 provedores**: Google grátis (sem chave), Claude, OpenAI, DeepL, Microsoft, MyMemory, LibreTranslate, Google API
- 🛡 **Proteção automática** de variáveis Ren'Py (`[var]`, `{tag}`, `%s`) — nada de `[fin]` virar `[barbatana]`
- 📖 **Glossário** — termos com tradução fixa ou que devem ser mantidos (nomes de personagens)
- 🧠 **Memória de tradução** persistente (SQLite) — reutiliza entre jogos
- ⚡ **Modo incremental** — não retraduz o que já tem
- 🔍 **Auto-detecção** de idioma do jogo (português, russo, japonês, etc.)
- 📦 **Suporte a `.rpa`** — extrai e descompila `.rpyc` embutidos
- 🎯 **Compatível com sistemas custom** — Fashion Business (`language_dict.json`), etc.
- 🔧 **Reparador** de traduções com variáveis quebradas
- 🚀 **Rápido** — Google Free com batch + paralelismo (3-5× mais rápido)

---

## 📸 Capturas de tela

![Texto Alternativo](https://i.imgur.com/TEjoxoUl.jpg)

---

## 🚀 Instalação

### Opção 1 — Executável pronto (recomendado)

1. Baixe a última versão em **[Releases](https://github.com/shedins/inkatranslate/releases)**
2. Extraia o `.zip` em qualquer pasta
3. Clique duas vezes em **`INKA-Tradutor.exe`**
4. O navegador abre em `http://localhost:5000`

### Opção 2 — Código-fonte (Python 3.10+)

```bash
git clone https://github.com/shedins/inkatranslate.git
cd inkatranslate
pip install -r requirements.txt
python app.py
```

---

## 📋 Uso básico

1. **Cole o caminho do jogo** (`.exe` ou pasta `game/`)
2. Clique **🔍 Escanear** — o app detecta automaticamente:
   - Idioma original do jogo
   - Arquivos `.rpy`, `.rpa`, `.rpyc`
   - Padrões customizados de variáveis
3. **Selecione o provedor**:
   - `Google Tradutor` — grátis, sem chave, padrão
   - `Claude` / `OpenAI` / `DeepL` — qualidade superior, precisa de API key
4. Escolha **Idioma alvo** (padrão: Português BR)
5. Clique **⚡ Iniciar tradução**

O resultado é salvo em `game/tl/pt_br/` (modo padrão Ren'Py).

---

## 🎯 Recursos avançados

### 📖 Glossário (proteção e tradução fixa de termos)

`game/glossary.json`:
```json
{
  "Eve": null,                  
  "Hometown": "Cidade Natal",   
  "Mom": "Mãe"
}
```
- `null` → mantém termo original
- string → tradução fixa garantida (100% consistente entre cenas)

### 🛡 Padrões protegidos (regex)

Built-in (sempre ativo): `[var]`, `{tag}`, `%(name)s`, `%s`, `%d`

Para jogos com sintaxe custom (`<<player>>`, `{{var}}`, `$name`, `%%var%%`):
- Aba **🛡 Padrões** → escolha um exemplo pronto ou adicione seu regex.

### 🎮 Sistemas custom (Fashion Business, etc.)

Se o jogo usa `language_dict.json`:
1. Aba **📦 language_dict** → cole o caminho do arquivo
2. **🔍 Analisar** — o app detecta automaticamente o `language_fields` do jogo
3. Marque **"Sobrescrever original"** e traduza
4. O app cria a pasta `tl/<engine_name>/` automaticamente para habilitar o botão de idioma no menu

### 🔧 Reparar traduções com variáveis quebradas

Já traduziu antes do INKA proteger variáveis? Use **🔧 Reparar variáveis** (painel esquerdo).

O app compara `old`/`new` em `tl/pt_br/` e restaura variáveis que foram traduzidas (ex: `[fin]` que virou `[barbatana]`).

### ⚡ Modo incremental

Re-rode a tradução no mesmo jogo — o INKA pula todos os textos já traduzidos em `tl/<lang>/`. Útil quando o jogo recebe atualizações.

### 🧠 Memória de tradução

Termos traduzidos uma vez ficam salvos em `data/translation_memory.db` e são reutilizados em outros jogos automaticamente. Aba **🧠 Memória** para gerenciar, exportar, importar.

---

## 📁 Estrutura

```
INKA-Tradutor/                    ← extraído do .zip
├── INKA-Tradutor.exe              ← executável (substituído a cada update)
└── data/                          ← PERSISTE entre atualizações
    ├── translation_cache.db
    ├── translation_memory.db
    ├── glossary.json
    ├── replaces.json
    └── protect_patterns.json
```

⚠️ **Não apague a pasta `data/`** ao atualizar — ela guarda seu glossário, memória e configs.

---

## ❓ FAQ

**Não acha arquivos `.rpy` no jogo**
- O jogo usa `.rpa` (compactado) ou `.rpyc` (compilado). Use **Extrair .rpa** e/ou **Descompilar .rpyc** no painel esquerdo.

**A tradução do Google ficou ruim em algum termo**
- Adicione ao **Glossário** ou em **Substituições pós-tradução** (aba 🔄).

**O jogo dá erro `name 'X' is not defined`**
- Uma variável Ren'Py foi traduzida. Use **🔧 Reparar variáveis** no painel esquerdo.

**Quero usar Claude/OpenAI**
- Vá em **⚙ Configurações** e cole sua API key.

**Como ativar o idioma traduzido no jogo**
- O INKA gera `language_switch.rpy` que adiciona um seletor de idioma na tela. Para jogos com sistema próprio (Fashion Business, etc.), o app detecta automaticamente.

---

## 🛠 Build próprio (para desenvolvedores)

Para gerar seu próprio `.exe`:
```bash
build.bat
```
Resultado em `dist/INKA-Tradutor-release/`.

Configure `__github_repo__` em [`app.py`](app.py) para apontar para seu fork antes de buildar.

---

## 🤝 Contribuir

Issues, PRs e sugestões são bem-vindos. Áreas que precisam de ajuda:
- Suporte a outros sistemas custom de tradução
- Adicionar mais provedores (Yandex, Papago, etc.)
- Testes automatizados
- Tradução da interface para outros idiomas

---

## 📜 Licença

MIT — veja [LICENSE](LICENSE).

Inclui [unrpyc](https://github.com/CensoredUsername/unrpyc) (BSD) e UnRen.bat (público) para extração de `.rpa` e descompilação de `.rpyc`.

---

## 💝 Créditos

Feito com IA + paciência. Se for útil, deixe uma ⭐ no [repo](https://github.com/shedins/inkatranslate).
