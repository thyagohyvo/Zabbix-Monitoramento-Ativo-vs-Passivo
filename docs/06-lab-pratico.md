# Caso 06 — Laboratório prático guiado

Objetivo: sair da teoria e configurar **um item passivo e um item ativo** no mesmo host, para sentir a diferença na prática.

## Pré-requisitos

- Um Zabbix Server 7.x já instalado e acessível.
- Um host Linux com Zabbix Agent 2 instalado, que você tenha acesso root/sudo.
- IP do Zabbix Server conhecido.

## Parte 1 — Item Passivo

1. No host monitorado, edite `/etc/zabbix/zabbix_agent2.conf` e confirme:
   ```ini
   Server=<IP_DO_SEU_ZABBIX_SERVER>
   ListenPort=10050
   ```
2. Reinicie o agent: `sudo systemctl restart zabbix-agent2`
3. Na interface do Zabbix, vá em `Data collection > Hosts > seu host > Items > Create item`:
   - Nome: `Teste Passivo - CPU Load`
   - Tipo: `Zabbix agent`
   - Key: `system.cpu.load`
   - Intervalo: `30s`
4. Aguarde ~1 minuto e confira em `Monitoring > Latest data` se o valor aparece.
5. **Desafio**: bloqueie temporariamente a porta 10050 no firewall do host e observe o erro que aparece no item (`Configuration > Hosts`, ícone vermelho de erro). Depois libere de novo e veja o item se recuperar.

## Parte 2 — Item Ativo

1. No mesmo host, edite o `.conf` e adicione:
   ```ini
   ServerActive=<IP_DO_SEU_ZABBIX_SERVER>:10051
   Hostname=<NOME_EXATO_DO_HOST_NO_ZABBIX>
   ```
2. Reinicie o agent novamente.
3. Crie um segundo item:
   - Nome: `Teste Ativo - Memória livre`
   - Tipo: `Zabbix agent (active)`
   - Key: `vm.memory.size[available]`
   - Intervalo: `30s`
4. Aguarde ~1-2 minutos (o primeiro ciclo ativo demora um pouco mais, pois inclui a etapa de "pedir configuração").
5. **Desafio**: altere o `Hostname` para algo levemente diferente do cadastrado (ex: mude uma letra), reinicie o agent, e observe que o item ativo para de receber dados — mesmo com o agent rodando normalmente. Isso reproduz de propósito o erro mais comum do [Caso 05](05-erros-comuns.md).

## Parte 3 — Comparar nos logs

Com os dois itens configurados, rode em paralelo:
```bash
sudo tail -f /var/log/zabbix/zabbix_agent2.log
```
Observe que:
- O item **passivo** não gera log de "iniciativa" do Agent — ele só responde quando "perguntado" (você não verá o Agent puxando conexão).
- O item **ativo** gera logs periódicos de "sending" e de refresh de configuração ativa.

## O que você deve levar desse laboratório

Se ao final você conseguir explicar, com suas próprias palavras, **por que o item ativo parou quando você errou o `Hostname`**, e **por que o item passivo parou quando você bloqueou a porta 10050**, você já entendeu a diferença de verdade — não só decorou a teoria.

## Voltando

- [README principal](../README.md)
- [Caso 01 — revisar o conceito central](01-conceito.md)
