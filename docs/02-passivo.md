# Caso 02 — Monitoramento Passivo na prática

## Como funciona, passo a passo

1. Você cria um **item** na interface do Zabbix e escolhe o tipo `Zabbix agent` (não `Zabbix agent (active)`).
2. O Zabbix Server, no intervalo configurado (ex: a cada 60s), abre uma conexão TCP até o IP do host, na porta `10050`.
3. O Agent responde com o valor da chave (`key`) pedida.
4. A conexão é fechada. Na próxima coleta, tudo se repete.

## Configuração mínima no Agent

No arquivo `zabbix_agent2.conf` do host monitorado:

```ini
Server=192.168.1.10          # IP do Zabbix Server (quem tem permissão de perguntar)
ListenPort=10050              # porta em que o agent escuta
```

O parâmetro `Server` aqui funciona como uma **lista de permissões**: só IPs listados podem fazer perguntas passivas ao Agent.

## Configuração do item no Zabbix Server

Ao criar o item:

- **Tipo**: `Zabbix agent`
- **Key**: ex. `system.cpu.load`
- **Intervalo de atualização**: ex. `1m`

## Fluxo de firewall

```
[Zabbix Server] ───── porta 10050 ────►  [Agent]
                 (Server precisa alcançar o Agent)
```

Isso significa: **o firewall do host monitorado precisa liberar entrada na porta 10050, vinda do IP do Server**.

## Quando o passivo é a escolha certa

- Ambientes com **poucos hosts** e rede estável entre Server e Agents.
- Quando você quer que o **Server tenha controle total** de quando cada item é coletado.
- Ambientes onde é mais fácil liberar firewall **de dentro para fora do datacenter** (Server acessando os hosts) do que o contrário.

## Limitação a ter em mente

Cada item passivo gera uma conexão TCP separada. Com **muitos hosts e muitos itens**, isso pode sobrecarregar os "pollers" do Zabbix Server (processo responsável por fazer essas perguntas). É por isso que ambientes grandes tendem a usar mais monitoramento ativo — assunto do próximo caso.

## Próximo passo

- [Caso 03 — Monitoramento Ativo na prática](03-ativo.md)
- [Caso 05 — Erros comuns de iniciante](05-erros-comuns.md)
