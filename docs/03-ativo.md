# Caso 03 - Monitoramento Ativo na prática

## Como funciona, passo a passo

1. Você cria um item com o tipo `Zabbix agent (active)`.
2. O **Agent**, periodicamente (padrão: a cada 2 minutos, configurável), conecta na porta `10051` do Server/Proxy e pergunta: *"quais itens eu devo monitorar e com qual frequência?"* - isso é a **configuração ativa**.
3. O Server/Proxy responde com a lista de itens e intervalos.
4. O Agent, então, coleta os valores localmente e **envia** (`push`) esses valores de volta para o Server/Proxy na mesma porta `10051`, no intervalo que foi informado para cada item.

## Configuração mínima no Agent

```ini
ServerActive=192.168.1.10:10051   # para onde o agent deve enviar dados
Hostname=meu-servidor-web-01       # precisa bater EXATAMENTE com o nome do host no Zabbix
```

Aqui está a pegadinha mais comum de iniciante: **`Hostname` precisa ser idêntico ao nome do host cadastrado na interface do Zabbix**, caractere por caractere. Se não bater, o Agent envia dados mas o Server não sabe para qual host associar - e nada aparece nos gráficos.

## Configuração do item no Zabbix Server

- **Tipo**: `Zabbix agent (active)`
- **Key**: ex. `vfs.fs.size[/,pfree]`
- **Intervalo de atualização**: ex. `1m`

## Fluxo de firewall

```
[Agent] ───── porta 10051 ────► [Zabbix Server / Proxy]
        (Agent precisa alcançar o Server)
```

Aqui é o **oposto do passivo**: quem precisa ter saída liberada é o host monitorado, em direção ao Server/Proxy.

## Quando o ativo é a escolha certa

- Ambientes com **muitos hosts** (dezenas, centenas, milhares) - reduz drasticamente a carga do Server, porque ele não precisa mais "perguntar" ativamente a cada host.
- Hosts em **redes que só permitem tráfego de saída** (ex: hosts atrás de NAT restritivo, cloud com regras de saída mais abertas que entrada).
- Ambientes distribuídos geograficamente, especialmente combinados com **Zabbix Proxy**.
- Quando você quer resiliência: se a rede cair momentaneamente, o Agent guarda os valores em buffer local e envia depois (dentro de um limite configurável).

## Limitação a ter em mente

Você perde o controle central rígido sobre "quando exatamente" cada coleta acontece - o Agent é quem dita o ritmo dentro do intervalo configurado. Isso raramente é um problema real, mas é uma diferença conceitual importante.

## Próximo passo

- [Caso 04 - Tabela comparativa e quando usar cada um](04-comparativo.md)
- [Caso 05 - Erros comuns de iniciante](05-erros-comuns.md)
