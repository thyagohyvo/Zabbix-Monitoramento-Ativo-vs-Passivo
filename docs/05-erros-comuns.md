# Caso 05 — Erros comuns de iniciante

## 1. "Configurei tudo certo mas não aparece nenhum dado" (Ativo)

**Causa mais provável**: o `Hostname` no `zabbix_agent2.conf` não é idêntico ao nome do host cadastrado no Zabbix (maiúscula/minúscula, espaço, acento — tudo conta).

**Como confirmar**: vá em `Reports > Latest data` ou verifique em `Data collection > Hosts` se o host aparece com ícone de agente **verde** (ZBX). Se estiver cinza, o Agent nunca conseguiu se anunciar.

## 2. "Timeout ao coletar item" (Passivo)

**Causa mais provável**: firewall bloqueando a porta `10050` no host monitorado, ou o parâmetro `Server=` no Agent não inclui o IP correto do Zabbix Server.

**Como confirmar**: no host monitorado, rode:
```bash
sudo ss -tuln | grep 10050
```
Se a porta não estiver em `LISTEN`, o serviço do Agent não está rodando ou está com erro de configuração.

## 3. Misturar `Server=` e `ServerActive=` sem entender a diferença

`Server=` controla **quem tem permissão de perguntar** (passivo). `ServerActive=` controla **para onde o Agent envia dados** (ativo). Você pode ter os dois configurados ao mesmo tempo, e é comum ter — mas cada um resolve um problema diferente. Configurar um pensando que resolve o outro é a origem de boa parte da confusão inicial.

## 4. Esquecer de reiniciar o serviço do Agent após editar o `.conf`

Mudanças no `zabbix_agent2.conf` só valem depois de:
```bash
sudo systemctl restart zabbix-agent2
```

## 5. Achar que "ativo" significa "tempo real"

Não significa. O intervalo de coleta ativo é configurado item a item, igual ao passivo. "Ativo" descreve *quem inicia a conexão*, não a frequência.

## 6. Testar conectividade do jeito errado

Para testar um item **passivo** manualmente a partir do Server:
```bash
zabbix_get -s <IP_DO_AGENT> -p 10050 -k system.cpu.load
```

Para investigar problemas em itens **ativos**, o teste é diferente — não existe um "zabbix_get ativo" direto, porque quem inicia é o Agent. Nesse caso, o caminho é olhar o log do Agent:
```bash
sudo tail -f /var/log/zabbix/zabbix_agent2.log
```
e procurar por linhas de "active check" ou erros de conexão com o `ServerActive`.

## Próximo passo

- [Caso 06 — Laboratório prático guiado](06-lab-pratico.md)
