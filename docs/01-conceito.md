# Caso 01 — O conceito central: quem pergunta e quem responde

## O problema de entendimento mais comum

A maioria dos iniciantes tenta entender "ativo vs passivo" pela **performance** ou pela **configuração**. Isso é um erro de ordem. Antes de qualquer coisa, entenda **quem inicia a conexão de rede**. Todo o resto decorre disso.

## Passivo: o Server pergunta

No monitoramento passivo, o **Zabbix Server (ou Proxy)** abre uma conexão TCP até o **Agent**, na porta `10050`, e pergunta o valor de um item específico. O Agent responde e a conexão se encerra.

- Quem inicia: **Server → Agent**
- Porta usada: `10050` (no Agent)
- O Agent é **passivo**: ele só fala quando é perguntado.

## Ativo: o Agent avisa

No monitoramento ativo, é o **Agent** quem abre a conexão até o **Server (ou Proxy)**, na porta `10051`, primeiro pedindo a lista de itens que ele deve monitorar (isso se chama "active check configuration request") e depois enviando os valores coletados nessa lista.

- Quem inicia: **Agent → Server**
- Porta usada: `10051` (no Server/Proxy)
- O Agent é **ativo**: ele decide quando falar, com base no intervalo configurado.

## Por que isso importa tanto?

Porque essa única diferença — quem abre a conexão — determina:

1. **Qual porta você precisa liberar no firewall**, e em qual sentido.
2. **Quem sofre com a latência de rede** quando ela existe.
3. **Como o sistema escala** quando você tem centenas ou milhares de hosts.
4. **O que aparece nos logs quando algo dá errado** — os erros são completamente diferentes entre os dois modos.

## Um erro comum de vocabulário

Muita gente confunde "item passivo" com "item lento" e "item ativo" com "item rápido". Isso **não tem relação nenhuma** com performance por si só. Ativo e passivo descrevem **direção da conexão**, não velocidade. A performance é uma consequência indireta, não a definição.

## Próximo passo

Agora que você tem o vocabulário fixado, siga para:

- [Caso 02 — Monitoramento Passivo na prática](02-passivo.md)
- [Caso 03 — Monitoramento Ativo na prática](03-ativo.md)
