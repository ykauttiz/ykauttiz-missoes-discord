<div align="center">

<!-- Banner -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=200&section=header&text=yKauttiz&fontSize=80&fontColor=fff&animation=twinkling&fontAlignY=35&desc=Miss%C3%B5es%20Discord%20%E2%80%94%20Script%20Gratuito%20STABLE%202026&descSize=20&descAlignY=60&descColor=aaa" width="100%"/>

<br/>

<!-- Badges row 1 -->
[![Version](https://img.shields.io/badge/version-v4.0.0-06b6d4?style=for-the-badge&logo=github&logoColor=white)](https://github.com/ykauttiz/ykauttiz-missoes-discord)
[![Status](https://img.shields.io/badge/status-STABLE-22c55e?style=for-the-badge&logo=checkmarx&logoColor=white)](#)
[![License](https://img.shields.io/badge/licen%C3%A7a-Copyright%202026-a855f7?style=for-the-badge&logo=shield&logoColor=white)](#copyright)
[![Discord](https://img.shields.io/badge/Servidor-discord.gg%2FG3wrqyJQrC-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/G3wrqyJQrC)

<!-- Badges row 2 -->
[![Free](https://img.shields.io/badge/100%25-Gratuito-22c55e?style=flat-square&logo=opensourceinitiative&logoColor=white)](#)
[![No Install](https://img.shields.io/badge/sem%20instala%C3%A7%C3%A3o-console%20only-06b6d4?style=flat-square&logo=javascript&logoColor=white)](#uso-r%C3%A1pido)
[![Anti-Detect](https://img.shields.io/badge/gaussiano-3%20camadas-a855f7?style=flat-square&logo=databricks&logoColor=white)](#destaques-t%C3%A9cnicos)
[![Anti-Hang](https://img.shields.io/badge/Anti--Hang-25min%20timeout-ef4444?style=flat-square&logo=shield&logoColor=white)](#)
[![Missions](https://img.shields.io/badge/tipos%20suportados-6-eab308?style=flat-square&logo=discord&logoColor=white)](#miss%C3%B5es-suportadas)

<br/>

> **⚠️ USE POR SUA CONTA E RISCO.**  
> Este script usa APIs internas não oficiais do Discord e pode violar os Termos de Serviço.  
> O autor não se responsabiliza por banimentos ou qualquer consequência.

<br/>

</div>

---

## 📋 Índice

- [O que é](#-o-que-é)
- [Missões suportadas](#-missões-suportadas)
- [Destaques técnicos](#-destaques-técnicos)
- [Como usar](#-como-usar)
- [Configuração](#-configuração-cfedit)
- [Comandos do console](#-comandos-do-console)
- [Anti-detecção](#-sistema-anti-detecção)
- [Changelog](#-changelog)
- [Compatibilidade](#-compatibilidade)
- [Copyright](#-copyright)
- [Suporte](#-suporte)

---

## 🎯 O que é

**yKauttiz Missões Discord** é um script JavaScript que roda diretamente no console do Discord (F12) e completa suas **missões (Quests)** automaticamente — sem instalar nada, sem executável, sem extensão.

```
Cole no console → pressione Enter → o script faz tudo
```

**Por que usar este e não outro?**

|  | yKauttiz v4 | Scripts genéricos |
|---|---|---|
| Retomada parcial | ✅ Continua de onde parou | ❌ Recomeça do zero |
| Anti-detecção | ✅ Gaussiano 3 camadas | ❌ Delays fixos |
| Travamento | ✅ Anti-Hang 25min | ❌ Trava para sempre |
| CONFIG editável | ✅ Bloco no topo | ❌ Hardcoded |
| Parada graciosa | ✅ `yKauttizStop()` | ❌ Força fechar |
| Log exportável | ✅ `window.yKauttizLog` | ❌ Sem histórico |
| Auto-enroll | ✅ Toggle opcional | ❌ Manual sempre |

---

## 🎮 Missões suportadas

<div align="center">

| Missão | Browser | Desktop App | Observação |
|--------|:-------:|:-----------:|------------|
| `WATCH_VIDEO` | ✅ | ✅ | Retomada parcial · step bi-modal gaussiano |
| `WATCH_VIDEO_MOBILE` | ✅ | ✅ | Parâmetros de cliente mobile simulados |
| `PLAY_ON_DESKTOP` | ❌ | ✅ | GameStore injection · PID realista · HIDE_ACTIVITY |
| `STREAM_ON_DESKTOP` | ❌ | ✅ | StreamStore injection · Anti-Hang 25min |
| `PLAY_ACTIVITY` | ✅ | ✅ | VoiceStateStore · canal atual do usuário |
| `ACHIEVEMENT_IN_ACTIVITY` | ✅ | ✅ | `location_type: voice_channel` · trata 403 |

</div>

> **Nota:** `PLAY_ON_DESKTOP` e `STREAM_ON_DESKTOP` exigem o **Discord Desktop App** — os stores necessários não estão disponíveis no navegador.

---

## ⚡ Destaques técnicos

<details>
<summary><b>🎲 Sistema gaussiano Box-Muller (3 camadas)</b></summary>
<br/>

Em vez de delays fixos — que são facilmente detectáveis — o script usa **distribuição gaussiana** em três camadas independentes:

```
Camada 1 — Timestamp enviado
  Cada valor de progresso tem drift por-request único.
  Não existe um offset fixo que crie fingerprint de sessão.

Camada 2 — Intervalo entre requests
  gaussian(1050ms × throttle, 200ms)
  Varia conforme a carga do servidor (smart throttle).

Camada 3 — Tamanho do step (bi-modal)
  80% → gaussian(5–9s) — assistindo normalmente
  20% → uniform(1–4s)  — pausa curta / lendo legenda
```

Resultado: padrão estatisticamente indistinguível de um humano assistindo um vídeo.

</details>

<details>
<summary><b>🛡️ Anti-Hang Protocol</b></summary>
<br/>

`PLAY_ON_DESKTOP` e `STREAM_ON_DESKTOP` esperam por eventos do Discord via `Dispatcher.subscribe`. Se o Discord parar de enviar heartbeats (crash, update, amigo sair do canal), o script travaria para sempre.

**Solução:**
```javascript
await Promise.race([questComplete, hangTimeout]);
// hangTimeout = 25 minutos (configurável via CFG.PLAY_TIMEOUT)
```

O `finally` sempre restaura `GameStore` e `StreamStore` — mesmo em caso de timeout ou erro.

</details>

<details>
<summary><b>🔄 Retomada parcial de progresso</b></summary>
<br/>

Antes de iniciar cada missão, o script lê o progresso real armazenado no QuestStore do Discord:

```javascript
fr = Math.floor(quest.userStatus?.progress?.[type]?.value ?? 0);
// configVersion 1 e 2 suportados corretamente
```

Se você assistiu 43s de um vídeo de 87s, o script começa de 43s — não repete o que já foi feito.

</details>

<details>
<summary><b>🔌 Multi-strategy webpack bootstrap</b></summary>
<br/>

O Discord atualiza internamente com frequência — o caminho do webpack muda entre versões. O script tem 3 estratégias de extração em cascata:

```javascript
const STRATS = [
  () => webpackChunkdiscord_app.push([[Symbol()], {}, r => r]),
  () => /* estratégia 2 */,
  () => /* estratégia 3 */,
];
// Tenta cada uma até encontrar WP.c válido
```

Cada store (QuestStore, API, Dispatcher, etc.) também tem fallback de caminho.

</details>

<details>
<summary><b>✅ Verificação robusta do servidor</b></summary>
<br/>

A verificação passou por 3 iterações e agora é a mais confiável:

```
1. MemberStore.isMember(GID)     → mais direto, mais confiável
2. GuildStore.getAllGuilds()      → fallback se MemberStore indisponível
3. POST /invites/G3wrqyJQrC      → tenta entrar se não for membro
4. Double-check em 400           → 400 pode ser "já membro" → verifica de novo
   Se ainda não for membro → para o script com 4 mensagens de erro
```

</details>

---

## 🚀 Como usar

### Passo 1 — Habilitar DevTools

O **Discord Stable** bloqueia o console por padrão. Se `F12` não abrir nada:

<details>
<summary><b>Windows</b></summary>

1. Feche o Discord completamente
2. Abra `%appdata%\discord\settings.json` em um editor de texto
3. Adicione a linha abaixo dentro do objeto JSON:
```json
"DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING": true
```
4. Salve e reinicie o Discord

</details>

<details>
<summary><b>Linux / macOS</b></summary>

1. Feche o Discord completamente
2. Abra `~/.config/discord/settings.json`
3. Adicione a mesma linha dentro do objeto JSON
4. Salve e reinicie o Discord

</details>

<details>
<summary><b>Alternativa mais simples</b></summary>

Use o **Discord PTB** ou **Discord Canary** — ambos têm DevTools habilitados por padrão e funcionam identicamente ao Stable para missões.

</details>

---

### Passo 2 — Inscreva-se nas missões

No Discord → aba **Missões** → clique em **Aceitar** em cada missão desejada.

> Missões de jogos específicos exigem o jogo **instalado e vinculado** à sua conta Discord. O script não consegue contornar essa validação.

---

### Passo 3 — Copie e cole o script

1. Acesse **[ykauttiz.github.io/ykauttiz-missoes-discord](https://ykauttiz.github.io/ykauttiz-missoes-discord)**
2. Clique em **"Copiar Script"**
3. No Discord, pressione `F12` → aba **Console**
4. Cole com `Ctrl+V` e pressione `Enter`

> Se aparecer **"allow pasting"** no console, **digite** `allow pasting` e pressione Enter antes de colar.

---

### Passo 4 — Aguarde e colete

O script exibe logs coloridos em tempo real:

```
 yKauttiz  Missões Discord  STABLE  v4.0.0  discord.gg/G3wrqyJQrC

  !! USE POR SUA CONTA E RISCO !!
  Sessão: yk_RGI7B5 | v4.0.0 | Drift: 134ms
  Para parar: yKauttizStop()

[yKauttiz] Verificando servidor (obrigatório)...
[yKauttiz] ✓ Já está no servidor yKauttiz! discord.gg/G3wrqyJQrC
[yKauttiz] 2 missão(ões) | ETA ~3min | yk_RGI7B5
  [1] Ready or Not DLC — WATCH_VIDEO | 43s/87s | expira 99h
[yKauttiz] "Ready or Not DLC": retomando de 43s

[yKauttiz] Ready or Not DLC
  [█████████░░░░░░░░░░░] 49%  43s/87s  ~1min  vel:6.8s/r normal
  [████████████████████] 100%  87s/87s  PRONTO
[yKauttiz] ✓ "Ready or Not DLC" concluída em 24s! vel:7.0s/step
```

Após terminar → aba **Missões** no Discord → colete recompensas e orbs manualmente.

---

## ⚙️ Configuração `CFG` (edit)

O bloco `CFG` fica no **topo do script** — edite antes de colar:

```javascript
const CFG = {
  // ── Velocidade dos vídeos ─────────────────────────────────────────
  VIDEO_SPEED: 'normal',     // 'rapido' (3-7s) | 'normal' (5-9s) | 'lento' (7-14s)

  // ── Privacidade ───────────────────────────────────────────────────
  HIDE_ACTIVITY: false,      // true = esconde "Jogando X" durante PLAY_ON_DESKTOP

  // ── Modo seguro ───────────────────────────────────────────────────
  DRY_RUN: false,            // true = lista missões e ETAs sem enviar requests

  // ── Logs ──────────────────────────────────────────────────────────
  LOG_LEVEL: 'normal',       // 'normal' | 'verbose' | 'silencioso'
  EXPORT_LOG: true,          // true = salva sessão em window.yKauttizLog

  // ── Fila de missões ───────────────────────────────────────────────
  URGENCY_FIRST: true,       // prioriza missões que expiram mais cedo
  SHUFFLE_QUEUE: true,       // embaralha para variar padrão entre sessões

  // ── Funcionalidades experimentais ────────────────────────────────
  AUTO_ENROLL: false,        // tenta inscrição automática (experimental)
  NOTIFICATIONS: true,       // notificações nativas do OS ao completar

  // ── Limites de retry ─────────────────────────────────────────────
  MAX_RETRIES: { r429: 4, r5xx: 3, rNet: 5 },

  // ── Anti-Hang ────────────────────────────────────────────────────
  PLAY_TIMEOUT: 25 * 60 * 1000,  // timeout PLAY/STREAM em ms (padrão: 25min)
};
```

---

## 💻 Comandos do console

| Comando | O que faz |
|---|---|
| `yKauttizStop()` | Para o script graciosamente — finaliza o step atual, restaura GameStore/StreamStore e exibe resumo |
| `copy(JSON.stringify(window.yKauttizLog))` | Exporta o log completo da sessão em JSON para o clipboard |
| `window.yKauttizLog` | Acessa o objeto de log da sessão diretamente |

---

## 🛡️ Sistema anti-detecção

```
Drift por-request      Cada request tem latência variável independente
                       Base: gaussian(120ms, 40ms) por sessão
                       Por request: gaussian(BASE, 18ms) clamp[40, 380ms]

Step bi-modal          80% → gaussian(5-9s)  assistindo normalmente
                       20% → uniform(1-4s)   pausa curta / lendo legenda

Shuffle de fila        Fisher-Yates na ordem das missões
                       Urgentes (< 4h) ficam no topo

Micro-pausas           15% chance a cada step
                       Cooldown mínimo 30s entre pausas consecutivas
                       Duração: gaussian(4000ms, 1400ms)

Smart throttle         5xx  → intervalo × 1.5 (máx 4×)
                       2xx  → reseta para 1×
                       429  → backoff exponencial gaussiano

PID realista           range 4096–24576 (PIDs típicos de jogos no Windows)
```

---

## 📦 Changelog

### `v4.0.0` — STABLE · Mar 2026

```diff
+ AUTO_ENROLL toggle com delay gaussiano 2-8s antes de inscrever
+ ACHIEVEMENT_IN_ACTIVITY suportado com location_type: voice_channel
+ 3 estratégias webpack bootstrap (resiliência a updates do Discord)
+ UserStore para identificar usuário correto no VoiceState
+ Dispatcher fallback: exports.h e exports.default
+ API fallback: exports.Bo e exports.default
+ Log exportável: window.yKauttizLog com toda a sessão em JSON
~ checkServer: double-check de membership antes de aceitar 400
~ Todos os caracteres Unicode > 127 removidos (bug de proteção XOR corrigido)
```

---

### `v3.0.0` — STABLE · Mar 2026

```diff
+ Loop iterativo while(queue) — sem recursão, sem stack overflow
+ Anti-Hang Protocol: Promise.race + timeout 25min em PLAY e STREAM
+ CONFIG block editável no topo do script
+ window.yKauttizStop() — parada graciosa, restaura todos os stores
+ Notificações nativas do OS ao completar cada missão
+ Distribuição bi-modal no videoStep (80% normal + 20% pausa)
+ Per-request drift — elimina fingerprinting de sessão
+ Fisher-Yates shuffle + urgency-first scheduling
+ VoiceStateStore — detecta canal atual do usuário (não o primeiro da lista)
+ ETA global calculado antes de iniciar
+ Backoff unificado: backoff(type, attempt, status)
+ DRY_RUN mode — simula sem enviar requests
+ configVersion 1 e 2 suportados em WATCH_VIDEO
+ maybePause com cooldown mínimo 30s
~ checkServer: double-check de membership antes de aceitar 400
~ PLAY_ACTIVITY: VoiceStateStore em vez do primeiro canal encontrado
~ MemberStore path corrigido
- Recursão em processNext() removida
```

---

### `v2.2` — BETA · Fev 2026

```diff
+ Verificação obrigatória do servidor discord.gg/G3wrqyJQrC
+ Auto-join automático ao iniciar o script
+ Código protegido (XOR triplo + djb2 + fragmentação em 4 partes)
+ Velocity tracker: ETA pela velocidade real dos steps
+ Smart throttle: ×1.5 em 5xx, reseta em 2xx, máx ×4
+ Session digest: hash 4 chars do timestamp
- ACHIEVEMENT_IN_ACTIVITY (403 permanente nessa versão)
```

---

### `v2.1` — BETA · Jan 2026

```diff
+ Retomada parcial: lê userStatus.progress antes de iniciar
+ Backoff exponencial em 429: 8000×2^n ms, máx 4 tentativas
+ Backoff crescente em erros de rede: 5000×n ms, máx 5 tentativas
+ Micro-pausas aleatórias: 15% chance/step
! Auto-enroll: retorna 400 em 100% dos casos (jogo não instalado)
! Claim automático: 400 persistente (processamento assíncrono)
```

---

### `v2.0` — ALPHA · Dez 2025

```diff
+ Distribuição gaussiana Box-Muller em 3 camadas
+ SESSION_DRIFT base por sessão
+ Session ID único por execução (yk_XXXXXX)
+ PLAY_ON_DESKTOP via GameStore injection
+ STREAM_ON_DESKTOP via StreamStore injection
+ Tratamento completo: 400, 401, 403, 404, 429, 5xx, rede
```

---

### `v1.2` — LEGACY · Nov 2025

```diff
+ Tentativa de store reload via API para atualizar progresso
+ Delay fixo 3000ms entre requests
! Store reload retorna 404 no Canary, instável no Stable
```

---

### `v1.1` — LEGACY · Nov 2025

```diff
+ Script base funcional para WATCH_VIDEO
+ Tentativa de auto-enroll via POST /quests/{id}/enroll
- Enroll: 400 em 100% dos casos
```

---

### `v1.0` — LEGACY · Out 2025

```diff
+ Primeira versão pública
+ WATCH_VIDEO via POST /video-progress
+ Logs coloridos básicos no console
+ QuestStore via webpack bootstrap
```

---

## 📊 Compatibilidade

<div align="center">

| Tipo | Browser | Discord App | Detalhes |
|------|:-------:|:-----------:|----------|
| `WATCH_VIDEO` | ✅ | ✅ | Retomada parcial · configVersion 1/2 · bi-modal |
| `WATCH_VIDEO_MOBILE` | ✅ | ✅ | Parâmetros mobile |
| `PLAY_ON_DESKTOP` | ❌ | ✅ | Requer GameStore · Anti-Hang · HIDE_ACTIVITY |
| `STREAM_ON_DESKTOP` | ❌ | ✅ | Requer StreamStore · 1 pessoa no canal |
| `PLAY_ACTIVITY` | ✅ | ✅ | VoiceStateStore · canal atual |
| `ACHIEVEMENT_IN_ACTIVITY` | ✅ | ✅ | voice_channel · trata 403 |

</div>

> 🟢 **Testado e compatível com Discord Stable · Mar 2026**

---

## ©️ Copyright

```
© 2026 yKauttiz — Todos os direitos reservados
```

<div align="center">

| ✅ Permitido | ❌ Proibido |
|---|---|
| Uso pessoal gratuito | Vender ou cobrar pelo script |
| Compartilhar **com crédito** e aviso de risco | Remover a proteção de autoria |
| Reportar bugs no servidor Discord | Distribuir versões modificadas |
| Sugerir melhorias | Uso comercial ou em escala |

</div>

> Versões legítimas **sempre** contêm: `yKauttiz` · `discord.gg/G3wrqyJQrC` · aviso de risco.  
> Versões sem esses elementos são não-autorizadas.

---

## 💬 Suporte

<div align="center">

[![Discord Server](https://img.shields.io/badge/Entrar%20no%20Servidor-discord.gg%2FG3wrqyJQrC-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/G3wrqyJQrC)

[![Website](https://img.shields.io/badge/Site%20Oficial-ykauttiz.github.io-06b6d4?style=for-the-badge&logo=github&logoColor=white)](https://ykauttiz.github.io/ykauttiz-missoes-discord)

</div>

- **Bugs:** abra uma issue ou reporte no servidor Discord
- **Novos tipos de missão:** quando o script encontrar um tipo desconhecido, ele copia automaticamente os detalhes para o clipboard — basta colar no servidor
- **Servidor ID:** `1356778105737580554`

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=100&section=footer" width="100%"/>

*Feito com ♥ por yKauttiz · **USE POR SUA CONTA E RISCO***

</div>
