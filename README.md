# yKauttiz Missões Discord — STABLE 2026

<div align="center">

![STABLE](https://img.shields.io/badge/versao-STABLE%202026-57F287?style=for-the-badge&labelColor=0d0d0d)
![GRATIS](https://img.shields.io/badge/preco-100%25%20gratuito-57F287?style=for-the-badge&labelColor=0d0d0d)
![PROTEGIDO](https://img.shields.io/badge/codigo-protegido-5865F2?style=for-the-badge&labelColor=0d0d0d)
![RISCO](https://img.shields.io/badge/⚠️-use%20por%20sua%20conta%20e%20risco-ED4245?style=for-the-badge&labelColor=0d0d0d)

**Automação de missões (Quests) do Discord via console do navegador**

[discord.gg/G3wrqyJQrC](https://discord.gg/G3wrqyJQrC) · © 2026 yKauttiz

</div>

---

## ⚠️ AVISO IMPORTANTE

> **USE POR SUA CONTA E RISCO.**
> Este script usa APIs internas do Discord de forma **não oficial** e pode violar os Termos de Serviço.
> yKauttiz **não** se responsabiliza por banimentos, suspensões, perda de conta ou qualquer outra consequência.
> Recomenda-se o uso em contas secundárias ou de teste.

---

## O que é

Script JavaScript que completa **missões do Discord (Quests)** automaticamente. Roda direto no console do Discord — não instala nada, não baixa nada, não acessa arquivos do computador. Tudo acontece dentro do navegador ou do aplicativo Discord que você já tem aberto.

O código aparece **protegido** (ilegível) para preservar a autoria de yKauttiz. Isso é prática comum entre desenvolvedores para proteger seu trabalho — não há nada malicioso. O script funciona normalmente ao colar no console.

---

## Missões suportadas

| Tipo | Descrição | Requisito |
|---|---|---|
| `WATCH_VIDEO` | Envia timestamps de progresso do vídeo | Navegador ou App |
| `WATCH_VIDEO_ON_MOBILE` | Igual ao anterior com parâmetros mobile | Navegador ou App |
| `PLAY_ON_DESKTOP` | Simula jogo rodando no Discord | **Desktop App obrigatório** |
| `STREAM_ON_DESKTOP` | Simula stream ativa | **Desktop App + 1 pessoa no canal** |
| `PLAY_ACTIVITY` | Heartbeat de atividade em canal de voz | Estar em canal de voz |

---

## Funcionalidades

- **Retomada de progresso parcial** — se o Discord mostra "faltam X segundos", o script detecta e continua de onde parou automaticamente
- **Timing anti-detecção** — delays com distribuição gaussiana simulam comportamento humano real
- **Velocity tracker** — ETA calculado pela velocidade média real dos steps
- **Smart throttle** — intervalo aumenta automaticamente se o servidor Discord estiver sobrecarregado
- **Todos os erros HTTP tratados** — 400, 401, 403, 404, 429, 5xx e erros de rede com estratégias individuais
- **Verificação de servidor** — entra em discord.gg/G3wrqyJQrC automaticamente
- **Fila automática** — processa todas as missões inscritas em sequência
- **Logs coloridos** — progresso visível em tempo real no console

---

## Como usar

### Pré-requisitos

- Discord aberto no **navegador** ou no **Desktop App**
- Para missões `PLAY_ON_DESKTOP` e `STREAM_ON_DESKTOP`: **Discord Desktop App** obrigatório
- **Inscrição manual** nas missões antes de rodar o script (o script não faz inscrição automática)

### Passo a passo

**1.** Abra o Discord e inscreva-se manualmente nas missões disponíveis na aba **Missões**

**2.** Abra o console do Discord:

| Sistema | Atalho |
|---|---|
| Windows / Linux | `F12` ou `Ctrl + Shift + I` |
| macOS | `Cmd + Opt + I` |

**3.** Clique na aba **Console** no topo do DevTools

**4.** Acesse `ykauttiz.github.io/ykauttiz-missoes-discord`, clique em Copiar Script e cole no console

> Se o Discord pedir `allow pasting` — digite isso no console e pressione Enter antes de colar o script

**5.** Acompanhe os logs coloridos. Ao terminar, abra a aba **Missões** no Discord e colete as recompensas manualmente

---

## Por que preciso fazer algumas coisas manualmente?

**Inscrição:** Missões de jogos específicos (WoW, Apex, Rocket League etc.) exigem que o jogo esteja instalado e vinculado à conta Discord. O Discord valida isso no servidor — impossível contornar via script.

**Coleta de recompensas:** O Discord processa recompensas de forma assíncrona com validação anti-fraude. Tentar coletar automaticamente retorna erro. As recompensas e orbs ficam disponíveis na aba Missões assim que o script termina.

---

## Isso é um vírus?

**Não.** O script é JavaScript puro que roda dentro do console do seu próprio Discord. Ele não baixa nada, não instala nada, não acessa arquivos do computador e não envia senhas ou dados pessoais a lugar nenhum. Quando você fecha o Discord, o script para e some — não fica rodando em segundo plano.

O código protegido é uma medida para preservar a autoria de yKauttiz, não para esconder algo malicioso.

---

## Erros conhecidos e tratamento

| Código | O que significa | O que o script faz |
|---|---|---|
| `400` | Bad Request — elegibilidade ou enroll | Pula a missão |
| `401` | Sessão expirada | Para o script — recarregue o Discord |
| `403` | Sem permissão (ex: ACHIEVEMENT requer cliente ativo) | Pula com explicação |
| `404` | Quest não encontrada / expirada | Pula |
| `429` | Rate limit | Backoff exponencial, máx 4 tentativas |
| `5xx` | Servidor Discord com problema | Smart throttle + retry, máx 3 tentativas |
| `0 / rede` | Falha de conexão | Backoff crescente, máx 5 tentativas |

---

## Histórico de versões

| Versão | Status | Descrição |
|---|---|---|
| **STABLE** | ✅ Atual | Retomada parcial, protegido, velocity tracker, smart throttle, todos os erros tratados |
| v2.2 | ⚠️ Beta | ACHIEVEMENT_IN_ACTIVITY (403 confirmado) |
| v2.1 | ⚠️ Beta instável | Enroll/claim tentados — ambos retornam 400 |
| v2.0 | 🔶 Alpha | Gaussiano puro, sem extras |
| v1.2 | 🔴 Legacy | Store reload (404 no Canary) |
| v1.0 | 🔴 Legacy | Script base original |

---

## Site

Acesse o site do projeto com documentação completa, log preview e FAQ:

```
https://ykauttiz.github.io/ykauttiz-missoes-discord
```

---

## Copyright

```
© 2026 yKauttiz · Todos os direitos reservados
discord.gg/G3wrqyJQrC · Servidor ID: 1356778105737580554
```

- ✅ Uso pessoal gratuito permitido
- ✅ Compartilhamento permitido **com crédito visível**
- ❌ Proibido vender ou monetizar
- ❌ Proibido remover autoria ou aviso de risco
- ❌ Proibido remover ou contornar a proteção do código
- ❌ Proibido redistribuir modificado sem identificar como fork

> Somente yKauttiz pode criar versões pagas deste script.

Versões legítimas **sempre** contêm: `yKauttiz` · `discord.gg/G3wrqyJQrC` · aviso de risco

---

<div align="center">

Feito com 💜 por **yKauttiz** · [discord.gg/G3wrqyJQrC](https://discord.gg/G3wrqyJQrC)

</div>
