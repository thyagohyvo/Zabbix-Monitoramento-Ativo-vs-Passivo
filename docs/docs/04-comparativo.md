# Caso 04 — Tabela comparativa e quando usar cada um

## Comparativo direto

| Aspecto | Passivo | Ativo |
|---|---|---|
| Quem inicia a conexão | Zabbix Server → Agent | Agent → Zabbix Server |
| Porta envolvida | `10050` (no Agent) | `10051` (no Server/Proxy) |
| Direção do firewall a liberar | Entrada no Agent | Entrada no Server/Proxy |
| Parâmetro chave no `.conf` do Agent | `Server=` | `ServerActive=` e `Hostname=` |
| Tipo de item na interface | `Zabbix agent` | `Zabbix agent (active)` |
| Escalabilidade (muitos hosts) | Pior — sobrecarrega pollers do Server | Melhor — distribui a carga |
| Bom para redes restritas por NAT/saída | Não | Sim |
| Resiliência a queda momentânea de rede | Baixa (a coleta simplesmente falha) | Alta (Agent guarda em buffer) |
| Controle central rígido do timing | Sim | Parcial |
| Complexidade inicial de configuração | Mais simples de entender | Exige atenção ao `Hostname` |

## Regra prática para decidir

- **Poucos hosts, rede simples, ambiente de laboratório/estudo** → comece com **passivo**. É mais fácil de visualizar o que está acontecendo.
- **Ambiente de produção com dezenas+ de hosts, ou hosts em nuvem/NAT** → prefira **ativo**.
- **Na prática real**, a maioria dos ambientes usa **os dois ao mesmo tempo**: itens de infraestrutura (CPU, disco, memória) costumam ir por ativo, e certos checks pontuais de rede (ex: verificar se uma porta específica está respondendo, executado sob demanda) vão por passivo.

## Não existe "melhor" absoluto

Essa é a armadilha mental mais comum: procurar qual dos dois é "o certo". Não é uma questão de qual é superior — é uma questão de **qual direção de conexão faz sentido na sua topologia de rede e no seu volume de hosts**. Volte ao [Caso 01](01-conceito.md) sempre que essa dúvida voltar.

## Próximo passo

- [Caso 05 — Erros comuns de iniciante](05-erros-comuns.md)
- [Caso 06 — Laboratório prático guiado](06-lab-pratico.md)
