# 📡 Zabbix: Monitoramento Ativo vs Passivo

Guia para quem está começando (ou travado) entendendo a diferença entre **checks ativos** e **checks passivos** no Zabbix.

---

## 🎯 Objetivo deste repositório

Se você já leu a documentação oficial do Zabbix sobre isso e saiu mais confuso do que entrou, esse repositório é pra você. Aqui a explicação é feita com analogias, exemplos de configuração reais e diagramas simples — sem assumir que você já entende o "zabbix-ês".

---

# 📋 Índice

| # | Tópico | Arquivo |
|---|--------|---------|
| 01 | [O conceito central: quem pergunta e quem responde](docs/01-conceito.md) | `docs/01-conceito.md` |
| 02 | [Monitoramento Passivo na prática](docs/02-passivo.md) | `docs/02-passivo.md` |
| 03 | [Monitoramento Ativo na prática](docs/03-ativo.md) | `docs/03-ativo.md` |
| 04 | [Tabela comparativa e quando usar cada um](docs/04-comparativo.md) | `docs/04-comparativo.md` |
| 05 | [Erros comuns de iniciante](docs/05-erros-comuns.md) | `docs/05-erros-comuns.md` |
| 06 | [Laboratório prático guiado](docs/06-lab-pratico.md) | `docs/06-lab-pratico.md` |

---

## 🧠 A analogia que resolve 80% da confusão

Antes de qualquer configuração, entenda isto — o resto do repositório é só detalhamento disso aqui:

> **Passivo** = o Zabbix Server **liga** para o Agent e pergunta "qual o valor disso agora?"
> **Ativo** = o Agent **liga** para o Zabbix Server (ou Proxy) e avisa "aqui está o valor disso".

Quem inicia a conexão é a diferença fundamental. Tudo o resto (portas, firewall, performance, escalabilidade) é consequência disso.

```
PASSIVO
┌───────────────┐   "me dá o valor de X"    ┌───────────┐
│ Zabbix Server │ ────────────────────────► │   Agent    │
│               │ ◄──────────────────────── │            │
└───────────────┘        resposta           └───────────┘

ATIVO
┌───────────────┐                            ┌───────────┐
│ Zabbix Server │ ◄──────────────────────── │   Agent    │
│  (ou Proxy)   │   "aqui vai o valor de X"  │            │
└───────────────┘                            └───────────┘
```

---

## 🚀 Como usar este repositório

Se você está travado, siga essa ordem:

1. Leia o **[Caso 01](docs/01-conceito.md)** primeiro, mesmo que já ache que entende — ele estabelece o vocabulário usado no resto do guia.
2. Vá para **Passivo** e **Ativo** separadamente. Não tente entender os dois ao mesmo tempo.
3. Só depois de entender cada um isoladamente, leia o **comparativo**.
4. Se algo já deu errado na sua configuração, pule direto para **erros comuns**.
5. Quer fixar o conteúdo com a mão na massa? Vá para o **laboratório prático**.

---

## 🛠️ Ambiente de referência

| Item | Versão / Detalhe |
|------|-------------------|
| Zabbix Server | 7.x |
| Agente | Zabbix Agent 2 |
| Banco de dados | MySQL / MariaDB |
| SO | Linux (Ubuntu/RHEL/Rocky) |

---

## 📂 Estrutura do repositório

```
zabbix-ativo-vs-passivo/
├── README.md                     ← você está aqui
└── docs/
    ├── 01-conceito.md
    ├── 02-passivo.md
    ├── 03-ativo.md
    ├── 04-comparativo.md
    ├── 05-erros-comuns.md
    └── 06-lab-pratico.md
```

---

## 🤝 Contribuindo

Ficou travado em algo que não está aqui? Abra uma [Issue](../../issues/new/choose) descrevendo onde você travou. Dúvida de iniciante é exatamente o tipo de conteúdo que esse repositório quer capturar.

> Mantido individualmente. Atualizado continuamente conforme novas dúvidas e casos de aprendizado surgem.
