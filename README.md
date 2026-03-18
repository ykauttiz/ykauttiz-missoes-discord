<div align="center">

<!-- Banner -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=200&section=header&text=yKauttiz&fontSize=80&fontColor=fff&animation=twinkling&fontAlignY=35&desc=Miss%C3%B5es%20Discord%20%E2%80%94%20Script%20Gratuito%20STABLE%202026&descSize=20&descAlignY=60&descColor=aaa" width="100%"/>

<br/>

[![Version](https://img.shields.io/badge/version-v5.0.0-06b6d4?style=for-the-badge&logo=github&logoColor=white)](https://github.com/ykauttiz/ykauttiz-missoes-discord)
[![Status](https://img.shields.io/badge/status-STABLE-22c55e?style=for-the-badge&logo=checkmarx&logoColor=white)](#)
[![License](https://img.shields.io/badge/licen%C3%A7a-Copyright%202026-a855f7?style=for-the-badge&logo=shield&logoColor=white)](#copyright)
[![Discord](https://img.shields.io/badge/Servidor-discord.gg%2FG3wrqyJQrC-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/G3wrqyJQrC)

[![Free](https://img.shields.io/badge/100%25-Gratuito-22c55e?style=flat-square&logo=opensourceinitiative&logoColor=white)](#)
[![No Install](https://img.shields.io/badge/sem%20instala%C3%A7%C3%A3o-console%20only-06b6d4?style=flat-square&logo=javascript&logoColor=white)](#como-usar)
[![Anti-Detect](https://img.shields.io/badge/gaussiano-3%20camadas-a855f7?style=flat-square&logo=databricks&logoColor=white)](#sistema-anti-detecção)
[![Anti-Hang](https://img.shields.io/badge/Anti--Hang-25min%20timeout-ef4444?style=flat-square&logo=shield&logoColor=white)](#)
[![Missions](https://img.shields.io/badge/tipos%20suportados-6-eab308?style=flat-square&logo=discord&logoColor=white)](#missões-suportadas)

<br/>

> **⚠️ USE POR SUA CONTA E RISCO.**  
> Este script usa APIs internas não oficiais do Discord e pode violar os Termos de Serviço.  
> O autor não se responsabiliza por banimentos ou qualquer consequência.

</div>

---

## 📋 Índice

- [O que é](#-o-que-é)
- [Missões suportadas](#-missões-suportadas)
- [Destaques técnicos](#-destaques-técnicos)
- [Como usar](#-como-usar)
- [Configuração CFG](#-configuração-cfg)
- [Comandos do console](#-comandos-do-console)
- [Sistema anti-detecção](#-sistema-anti-detecção)
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

|  | yKauttiz v5 | Scripts genéricos |
|---|---|---|
| Retomada parcial | ✅ Continua de onde parou | ❌ Recomeça do zero |
| Anti-detecção | ✅ Gaussiano 3 camadas | ❌ Delays fixos |
| Travamento | ✅ Anti-Hang 25min | ❌ Trava para sempre |
| CONFIG editável | ✅ Bloco no topo | ❌ Hardcoded |
| Parada graciosa | ✅ `yKStop()` | ❌ Força fechar |
| Log exportável | ✅ `window.yKauttizLog` | ❌ Sem histórico |
| Vídeos paralelos | ✅ Até 2 simultâneos | ❌ Apenas serial |
| Modo seguro | ✅ `SAFE_MODE` | ❌ Parâmetros fixos |
| Métricas | ✅ `yKMetrics()` | ❌ Sem estatísticas |

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

> **Nota:** `PLAY_ON_DESKTOP` e `STREAM_ON_DESKTOP` exigem o **Discord Desktop App**.

---

## ⚡ Destaques técnicos

<details>
<summary><b>🎲 Sistema gaussiano Box-Muller (3 camadas)</b></summary>
<br/>

```
Camada 1 — Timestamp enviado
  Drift por-request único: gaussian(BASE, 18ms) clamp[40, 380ms]
  Nenhum offset fixo → sem fingerprint de sessão

Camada 2 — Intervalo entre requests
  gaussian(1050ms × throttle × attentionPhase, 200ms)

Camada 3 — Tamanho do step (bi-modal)
  80% → gaussian(5–9s) — assistindo normalmente
  20% → uniform(1–4s)  — pausa curta / lendo legenda
```

</details>

<details>
<summary><b>🧠 Attention Phase Simulator (novo em v5)</b></summary>
<br/>

A cada 2 minutos, recalcula uma fase de atenção que modula o intervalo:

```
70% → gaussian(1.0, 0.3)  — focado
30% → gaussian(1.5, 0.4)  — distraído
clamp: [0.6, 2.5]
```

Simula o comportamento real de alternância de atenção humana.

</details>

<details>
<summary><b>🛡️ Anti-Hang Protocol</b></summary>
<br/>

```javascript
await Promise.race([questComplete, hangTimeout]);
// hangTimeout = 25 minutos (CFG.PLAY_TIMEOUT)
// finally: restaura GameStore e StreamStore sempre
```

</details>

<details>
<summary><b>⚡ Vídeos paralelos (novo em v5)</b></summary>
<br/>

Com `CFG.PARALLEL_VIDEO = true`, executa até 2 vídeos simultaneamente. Um jitter de 800ms–2500ms é adicionado para dessincronizar os requests e evitar rate limiting.

</details>

<details>
<summary><b>🔌 Multi-strategy webpack bootstrap</b></summary>
<br/>

3 estratégias de extração em cascata, resiliente a updates internos do Discord. Cada store também tem fallback de caminho.

</details>

---

## 🚀 Como usar

### Passo 1 — Habilitar DevTools

<details>
<summary><b>Windows</b></summary>

1. Feche o Discord completamente
2. Abra `%appdata%\discord\settings.json`
3. Adicione dentro do JSON:
```json
"DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING": true
```
4. Salve e reinicie

</details>

<details>
<summary><b>Linux / macOS</b></summary>

1. Feche o Discord
2. Abra `~/.config/discord/settings.json`
3. Adicione a mesma linha e reinicie

</details>

<details>
<summary><b>Alternativa</b></summary>

Use **Discord PTB** ou **Canary** — DevTools habilitado por padrão.

</details>

### Passo 2 — Inscreva-se nas missões

Discord → aba **Missões** → **Aceitar** em cada missão desejada.

### Passo 3 — Copie e cole

1. Acesse **[ykauttiz.github.io/ykauttiz-missoes-discord](https://ykauttiz.github.io/ykauttiz-missoes-discord)**
2. Clique em **"Copiar Script"**
3. Discord → `F12` → aba **Console** → cole → `Enter`

> Se aparecer **"allow pasting"**, **digite** `allow pasting` e Enter antes de colar.

### Passo 4 — Aguarde e colete

```
 yKauttiz  Missoes Discord  v5.0.0  discord.gg/G3wrqyJQrC

  !! USE POR SUA CONTA E RISCO !!
  Sessao: yk_RGI7B5 | v5.0.0 | Discord Stable | Desktop:Nao
  Comandos: yKStop() | yKStatus() | yKLog() | yKMetrics()

[yK] Verificando servidor (obrigatorio)...
[yK] [OK] Servidor verificado! discord.gg/G3wrqyJQrC
[yK] 2 missao(oes) | ETA ~3min | yk_RGI7B5
  [1] Ready or Not DLC   WATCH_VIDEO | 43s/87s | expira em 99h
[yK] "Ready or Not DLC": retomando de 43s (49%)
[yK] [OK] "Ready or Not DLC" concluida em 24s! vel:7.0s/step
```

Após terminar → aba **Missões** → colete recompensas manualmente.

---

## ⚙️ Configuração CFG

```javascript
const CFG = {
  VIDEO_SPEED: 'normal',     // 'rapido' (3-7s) | 'normal' (5-9s) | 'lento' (7-14s)
  HIDE_ACTIVITY: false,      // esconde "Jogando X" durante PLAY_ON_DESKTOP
  DRY_RUN: false,            // lista missões e ETAs sem enviar requests
  SAFE_MODE: false,          // delays maiores — recomendado para contas principais
  LOG_LEVEL: 'normal',       // 'silent' | 'normal' | 'verbose' | 'debug'
  NOTIFICATIONS: true,       // notificações nativas do OS
  URGENCY_FIRST: true,       // missões que expiram antes ficam no topo
  SHUFFLE_QUEUE: true,       // embaralha fila para variar padrão
  AUTO_ENROLL: false,        // inscrição automática (experimental)
  PARALLEL_VIDEO: false,     // até 2 vídeos simultâneos (experimental)
  MAX_RETRIES: { r429: 4, r5xx: 3, rNet: 5 },
  WATCHDOG_MS: 30 * 60 * 1000,  // timeout geral por missão (30min)
  PLAY_TIMEOUT: 25 * 60 * 1000, // timeout Anti-Hang PLAY/STREAM (25min)
};
```

---

## 💻 Comandos do console

| Comando | O que faz |
|---|---|
| `yKStop()` | Para graciosamente — finaliza step atual, restaura stores, exibe resumo |
| `yKStatus()` | Status em tempo real: concluídas, erros, tempo decorrido |
| `yKLog()` | Últimas 20 entradas do log estruturado |
| `yKMetrics()` | Métricas por tipo de missão (avg, ok, skip, erro) |
| `copy(JSON.stringify(window.yKauttizLog))` | Exporta log completo em JSON |

> `yKauttizStop()` mantido como alias para compatibilidade com v4.

---

## 🛡️ Sistema anti-detecção

```
Drift por-request      gaussian(BASE, 18ms) clamp[40, 380ms] — único por request
Attention Phase        Recalculada a cada 2min — [0.6x, 2.5x] do intervalo base
Step bi-modal          80% gaussian(5-9s) + 20% uniform(1-4s)
Micro-pausas           15% chance/step — cooldown 30s — gaussian(4000ms, 1400ms)
Pausas longas          3% chance/video — 2-5min — max 1x a cada 10min
Shuffle de fila        Fisher-Yates — urgentes (<4h) no topo
Smart throttle         5xx ×1.5 (max 4×) · 429 backoff exponencial
PID realista           range 4096–24576
Jitter paralelo        800ms–2500ms entre videos paralelos
```

---

## 📦 Changelog

### `v5.0.0` — STABLE · Mar 2026

```diff
+ PARALLEL_VIDEO: 2 videos simultaneos com jitter dessincronizador
+ SAFE_MODE: delays maiores para contas que precisam de mais cautela
+ AttentionPhase Simulator: modula interval a cada 2min (focado/distraido)
+ Pausas longas: 3% chance, 2-5min, simula break humano real
+ Event Emitter (EV): arquitetura orientada a eventos
+ Metrics engine: stats por tipo de missao (yKMetrics())
+ yKStatus(): status em tempo real sem parar o script
+ yKLog(): ultimas 20 entradas do log estruturado
+ Watchdog por missao (30min independente do PLAY_TIMEOUT)
+ Mission class com state machine completa
+ _errReason: rastreia razao real de erro (401 vs outros)
~ BUG: deteccao de 401 no loop serial era sempre true (corrigido)
~ BUG: m.from nao era atualizado em runActivity (corrigido)
~ BUG: longPause definida mas nunca chamada (corrigido)
~ BUG: videos paralelos sem jitter causavam rate limit sincronizado (corrigido)
~ BUG: flush final enviado mesmo longe do target (corrigido — so >= 99%)
~ BUG: sanitizacao de exe path incompleta (corrigido)
~ LOG.summary com padding dinamico (nomes longos nao quebram mais)
~ Watchdog agora para o loop em runActivity quando dispara
```

### `v4.0.0` — STABLE · Mar 2026

```diff
+ AUTO_ENROLL toggle com delay gaussiano 2-8s
+ ACHIEVEMENT_IN_ACTIVITY com location_type: voice_channel
+ 3 estrategias webpack bootstrap
+ UserStore para identificar usuario correto no VoiceState
+ window.yKauttizLog exportavel
~ checkServer: double-check antes de aceitar 400
```

### `v3.0.0` — STABLE · Mar 2026

```diff
+ Loop iterativo (sem recursao)
+ Anti-Hang Protocol: Promise.race + 25min timeout
+ CONFIG block editavel
+ yKauttizStop() — parada gracosa
+ Distribuicao bi-modal no videoStep
+ Per-request drift
+ Fisher-Yates shuffle + urgency-first
+ VoiceStateStore para PLAY_ACTIVITY
+ DRY_RUN mode
- Recursao em processNext() removida
```

### `v2.x / v1.x` — LEGACY · Out 2025–Fev 2026

```diff
v2.2: Servidor obrigatorio + smart throttle + session ID
v2.1: Retomada parcial + backoffs
v2.0: Gaussiano Box-Muller + GameStore + StreamStore injection
v1.2: Delay fixo 3000ms
v1.0: Primeira versao publica — WATCH_VIDEO basico
```

---

## 📊 Compatibilidade

> 🟢 **Testado e compatível com Discord Stable · Mar 2026**

| Tipo | Browser | Desktop App | Detalhes |
|------|:-------:|:-----------:|----------|
| `WATCH_VIDEO` | ✅ | ✅ | Retomada · configVersion 1/2 · bi-modal |
| `WATCH_VIDEO_MOBILE` | ✅ | ✅ | Parâmetros mobile |
| `PLAY_ON_DESKTOP` | ❌ | ✅ | GameStore · Anti-Hang · HIDE_ACTIVITY |
| `STREAM_ON_DESKTOP` | ❌ | ✅ | StreamStore · 1 pessoa no canal |
| `PLAY_ACTIVITY` | ✅ | ✅ | VoiceStateStore · canal atual |
| `ACHIEVEMENT_IN_ACTIVITY` | ✅ | ✅ | voice_channel · trata 403 |

---

## ©️ Copyright

```
© 2026 yKauttiz — Todos os direitos reservados
```

| ✅ Permitido | ❌ Proibido |
|---|---|
| Uso pessoal gratuito | Vender ou cobrar pelo script |
| Compartilhar **com crédito** e aviso de risco | Remover autoria |
| Reportar bugs | Distribuir versões modificadas |

> Versões legítimas **sempre** contêm: `yKauttiz` · `discord.gg/G3wrqyJQrC` · aviso de risco.

---

## 💬 Suporte

<div align="center">

[![Discord](https://img.shields.io/badge/Entrar%20no%20Servidor-discord.gg%2FG3wrqyJQrC-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/G3wrqyJQrC)
[![Website](https://img.shields.io/badge/Site%20Oficial-ykauttiz.github.io-06b6d4?style=for-the-badge&logo=github&logoColor=white)](https://ykauttiz.github.io/ykauttiz-missoes-discord)

</div>

- **Bugs:** issue ou servidor Discord
- **Tipo desconhecido:** o script copia os detalhes para clipboard automaticamente — cole no servidor
- **Servidor ID:** `1356778105737580554`

---

<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=100&section=footer" width="100%"/>

*Feito com ♥ por yKauttiz · **USE POR SUA CONTA E RISCO***
</div>
