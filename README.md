# yKauttiz Missões Discord

> Script gratuito que automatiza missões do Discord diretamente pelo console do navegador/app.  
> **v4.0.0 STABLE · © 2026 yKauttiz · USE POR SUA CONTA E RISCO**

---

## ⚠️ Aviso importante

Este script usa APIs internas não oficiais do Discord e pode violar os Termos de Serviço.  
**USE EXCLUSIVAMENTE POR SUA CONTA E RISCO. O autor não se responsabiliza por banimentos, suspensões ou qualquer consequência.**

---

## O que é

Um script JavaScript que roda no console do Discord (F12) e completa suas missões (Quests) automaticamente:

- **WATCH_VIDEO** — assiste vídeos de missão com timing gaussiano variável
- **WATCH_VIDEO_MOBILE** — idem, com parâmetros de cliente mobile
- **PLAY_ON_DESKTOP** — simula jogo rodando no Discord Desktop App
- **STREAM_ON_DESKTOP** — simula stream ativa com 1 pessoa no canal de voz
- **PLAY_ACTIVITY** — envia heartbeats para missões de atividade
- **ACHIEVEMENT_IN_ACTIVITY** — suporte a missões de conquista em canal de voz

---

## Destaques técnicos v4.0.0

| Feature | Descrição |
|---|---|
| **Gaussiano bi-modal** | 80% normal (5-9s) + 20% pausas curtas (1-4s) por step |
| **Per-request drift** | Cada request tem latência variável independente — sem fingerprinting |
| **Retomada parcial** | Lê progresso real do QuestStore antes de começar — nunca repete |
| **Anti-Hang Protocol** | `Promise.race` com timeout de 25min em PLAY e STREAM |
| **Loop iterativo** | `while(queue)` em vez de recursão — sem risco de stack overflow |
| **Auto-enroll** | Inscrição automática com delay gaussiano 2-8s (toggle via CONFIG) |
| **Smart throttle** | Multiplica intervalo em erros 5xx, reseta em sucesso |
| **3 estratégias webpack** | Fallback para múltiplos builds do Discord (Stable/PTB/Canary) |
| **VoiceStateStore** | Detecta canal onde o usuário está, não o primeiro encontrado |
| **yKauttizStop()** | Para o script graciosamente, restaura todos os stores |
| **yKauttizLog** | Log de sessão exportável em JSON |

---

## Uso rápido

### 1. Habilitar DevTools (se necessário)

O Discord Stable bloqueia o console por padrão. Para habilitar:

**Windows:** Abra `%appdata%/discord/settings.json` e adicione:
```json
"DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING": true
```

**Linux/macOS:** Mesmo processo em `~/.config/discord/settings.json`

**Alternativa:** Use Discord PTB ou Canary (DevTools habilitados por padrão).

### 2. Inscreva-se nas missões

No Discord → aba **Missões** → inscreva-se manualmente nas elegíveis.  
Missões de jogos específicos exigem o jogo instalado e vinculado à conta.

### 3. Abra o console

- Windows/Linux: `F12` ou `Ctrl+Shift+I`  
- macOS: `Cmd+Opt+I`

### 4. Cole e pressione Enter

Copie o script em [ykauttiz.github.io/ykauttiz-missoes-discord](https://ykauttiz.github.io/ykauttiz-missoes-discord), cole no console e pressione Enter.

O script:
1. Verifica/entra no servidor oficial
2. Lista as missões ativas com ETA
3. Processa cada missão automaticamente
4. Exibe logs coloridos em tempo real

### 5. Colete as recompensas

Após o script terminar → aba **Missões** no Discord → colete manualmente.  
A coleta automática não funciona por design da plataforma.

---

## Configuração

O script tem um bloco `CFG` editável no topo antes de colar:

```javascript
const CFG = {
  VIDEO_SPEED: 'normal',    // 'rapido' | 'normal' | 'lento'
  HIDE_ACTIVITY: false,     // esconde "Jogando X" durante PLAY_ON_DESKTOP
  DRY_RUN: false,           // lista missões sem enviar requests
  LOG_LEVEL: 'normal',      // 'normal' | 'verbose' | 'silencioso'
  URGENCY_FIRST: true,      // missões que expiram primeiro têm prioridade
  SHUFFLE_QUEUE: true,      // embaralha ordem para variar padrão
  AUTO_ENROLL: false,       // tenta inscrição automática (experimental)
  NOTIFICATIONS: true,      // notificações nativas ao completar
  EXPORT_LOG: true,         // salva log em window.yKauttizLog
};
```

---

## Comandos do console

| Comando | Descrição |
|---|---|
| `yKauttizStop()` | Para o script graciosamente |
| `copy(JSON.stringify(window.yKauttizLog))` | Copia o log de sessão |

---

## Perguntas frequentes

**Isso é malware?**  
Não. É JavaScript puro que roda no console do seu próprio Discord. Não baixa nada, não instala nada, não envia dados pessoais.

**Por que o código parece ilegível?**  
O script é distribuído com proteção de autoria para evitar redistribuição não autorizada. O Discord executa normalmente.

**Por que preciso estar no servidor?**  
O servidor é o canal oficial de suporte e atualizações. O script entra automaticamente ao iniciar.

**Posso ser banido?**  
USE POR SUA CONTA E RISCO. O script usa APIs internas não oficiais.

**Como para o script no meio?**  
Digite `yKauttizStop()` no console.

**Por que preciso coletar manualmente?**  
Coleta automática retorna 400 — processamento assíncrono do próprio Discord.

---

## Changelog

### v4.0.0 STABLE — Mar 2026
- `+` AUTO_ENROLL toggle com delay gaussiano 2-8s
- `+` ACHIEVEMENT_IN_ACTIVITY suportado com `location_type: voice_channel`
- `+` 3 estratégias webpack bootstrap para resiliência a updates
- `+` UserStore para identificação correta do usuário no VoiceState
- `+` Dispatcher e API com fallback de exports
- `+` Log exportável via `window.yKauttizLog`
- `~` checkServer: double-check de membership antes de aceitar 400
- `~` Todos os caracteres não-ASCII removidos do script (bug de proteção corrigido)

### v3.0.0 STABLE — Mar 2026
- `+` Loop iterativo (fim da recursão — sem stack overflow)
- `+` Anti-Hang Protocol: `Promise.race` + timeout 25min
- `+` CONFIG block editável no topo
- `+` Notificações nativas desktop
- `+` `window.yKauttizStop()` — parada graciosa
- `+` Distribuição bi-modal no videoStep
- `+` Per-request drift (elimina fingerprinting)
- `+` Fisher-Yates shuffle + urgency-first scheduling
- `+` VoiceStateStore para PLAY_ACTIVITY
- `+` ETA global
- `+` Backoff unificado
- `~` configVersion 1 e 2 suportados em WATCH_VIDEO
- `~` maybePause com cooldown mínimo 30s
- `-` Recursão em processNext() removida

### v2.2 BETA — Fev 2026
- `+` Verificação obrigatória do servidor
- `+` Auto-join automático ao iniciar
- `+` Código protegido (XOR triplo + djb2 + fragmentação)
- `+` Velocity tracker
- `+` Smart throttle (1.5× em 5xx, máx 4×)
- `+` Session digest
- `-` ACHIEVEMENT_IN_ACTIVITY removido (403 permanente nessa versão)

### v2.1 BETA — Jan 2026
- `+` Retomada de progresso parcial
- `+` Backoff exponencial em 429
- `+` Backoff crescente em erros de rede
- `+` Micro-pausas aleatórias (15% chance/step)

### v2.0 ALPHA — Dez 2025
- `+` Distribuição gaussiana Box-Muller em 3 camadas
- `+` SESSION_DRIFT base por sessão
- `+` Session ID único (yk_XXXXXX)
- `+` PLAY_ON_DESKTOP via GameStore
- `+` STREAM_ON_DESKTOP via StreamStore
- `+` Tratamento completo de erros HTTP

### v1.2 LEGACY — Nov 2025
- `+` Store reload via API
- `+` Delay fixo 3000ms
- `-` Store reload retorna 404 no Canary

### v1.1 LEGACY — Nov 2025
- `+` Script base WATCH_VIDEO
- `+` Auto-enroll tentado
- `-` Enroll retorna 400 em 100% dos casos

### v1.0 LEGACY — Out 2025
- `+` Primeira versão pública
- `+` WATCH_VIDEO via POST /video-progress
- `+` Logs coloridos
- `+` QuestStore via webpack bootstrap

---

## Compatibilidade

| Tipo | Navegador | Desktop App | Observação |
|---|---|---|---|
| WATCH_VIDEO | ✓ | ✓ | Retomada parcial · configVersion 1/2 |
| WATCH_VIDEO_MOBILE | ✓ | ✓ | Parâmetros mobile |
| PLAY_ON_DESKTOP | ✗ | ✓ | GameStore · Anti-Hang 25min |
| STREAM_ON_DESKTOP | ✗ | ✓ | StreamStore · 1 pessoa no canal |
| PLAY_ACTIVITY | ✓ | ✓ | VoiceStateStore |
| ACHIEVEMENT_IN_ACTIVITY | ✓ | ✓ | Canal de voz · trata 403 |

---

## Direitos autorais e licença

© 2026 yKauttiz — Todos os direitos reservados.

| ✅ Permitido | ❌ Proibido |
|---|---|
| Uso pessoal gratuito | Vender ou cobrar pelo script |
| Compartilhar com crédito e aviso de risco | Remover a proteção de autoria |
| Reportar bugs e sugerir melhorias | Distribuir versões modificadas sem autorização |
| | Uso comercial ou em escala |

Versões legítimas **sempre** contêm: `yKauttiz` · `discord.gg/G3wrqyJQrC` · aviso de risco.

---

## Suporte

- **Discord:** [discord.gg/G3wrqyJQrC](https://discord.gg/G3wrqyJQrC)
- **Servidor ID:** 1356778105737580554
- **Site:** [ykauttiz.github.io/ykauttiz-missoes-discord](https://ykauttiz.github.io/ykauttiz-missoes-discord)

---

*Feito com ♥ pela comunidade · USE POR SUA CONTA E RISCO*
