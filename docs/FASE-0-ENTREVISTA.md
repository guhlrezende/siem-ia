# Fase 0: roteiro de entrevista e mapa de concorrentes

Versão 0.1 · 2026-11-05 · Rascunho para Gustavo e Davi revisarem

Objetivo: descobrir se a dor existe e se alguém paga, antes de codar. Pronto quando 3 pessoas dizem que pagariam algo (valor anotado) e a dor principal está confirmada ou corrigida (critério do `PLANO.md`).

## Regras da conversa
- Meta de 8 a 10 entrevistas de 20 a 30 minutos, com dono, gestor ou TI de PME (10 a 200 funcionários).
- Ouvir mais do que falar. Não vender o produto nem mostrar a solução antes do bloco 4.
- Perguntar sobre o que já aconteceu, não sobre o que a pessoa "acha que faria". Fato passado vale mais que opinião.
- Anotar o valor exato que a pessoa disser, com as palavras dela.
- Não pedir nem receber log, senha ou dado real da empresa na entrevista.
- Se a pessoa for cliente do Davi, ele conduz; o Gustavo anota.

## Roteiro

**Bloco 1. Contexto (3 min)**
1. Quantas pessoas e quantas máquinas/servidores a empresa tem? Algo na nuvem?
2. Quem cuida de TI e segurança? É dedicado ou acumula função?

**Bloco 2. Situação atual (7 min)**
3. O que vocês usam hoje para proteger e monitorar (antivírus, firewall, backup, algum SIEM)? Quanto custa por mês?
4. Se alguém invadisse a empresa hoje à noite, como vocês ficariam sabendo? Quanto tempo levaria?
5. Quem recebe os avisos do que existe hoje, e o que faz com eles?

**Bloco 3. Histórico e dor (7 min)**
6. Já tiveram algum incidente (ransomware, e-mail falso, conta invadida, vazamento)? O que aconteceu e quanto custou, em dinheiro ou em tempo parado?
7. Já deixaram de olhar um alerta importante por excesso de aviso? Como foi?
8. O que mais tira o sono nessa área?

**Bloco 4. Reação à ideia (5 min), só agora**
9. Descrição curta: "um serviço que monitora os computadores e manda no celular um aviso já explicado em português, dizendo o que houve, a probabilidade de ser ataque de verdade e o que fazer". Isso faria sentido para vocês?
10. O que faria você não confiar nisso?
11. Quanto isso valeria por mês? Qual valor pareceria barato demais (e desconfiável)? E caro demais?
12. Preferem cobrança por máquina ou por empresa?

**Bloco 5. Fechamento (3 min)**
13. Conhece mais alguém que tenha essa dor e toparia conversar?
14. Posso voltar a falar com você quando houver algo para testar?

## Ficha por entrevista (copiar para cada pessoa)

| Campo | Resposta |
|---|---|
| Data / quem conduziu | |
| Empresa (porte, setor), sem dado sensível | |
| Cargo do entrevistado | |
| O que usa hoje e quanto paga | |
| Já teve incidente? Custo | |
| Dor principal, nas palavras dela | |
| Pagaria? Valor anotado | |
| Preferência de cobrança | |
| Objeção principal | |
| Indicou alguém? | |

## Mapa de concorrentes (a preencher com fonte e data)

Nada abaixo foi pesquisado ainda. Cada linha precisa de link da fonte e data da consulta antes de entrar em qualquer decisão.

| Concorrente | Tipo | Preço (fonte, data) | Público | O que não faz |
|---|---|---|---|---|
| Serviços gerenciados de Wazuh (brasileiros) | Gerenciado | | | |
| MDR para PME | Gerenciado | | | |
| Microsoft Sentinel (módulos de IA) | SIEM grande | | | |
| Google SecOps | SIEM grande | | | |
| EDR/antivírus com alerta no celular | Endpoint | | | |

## Para decidir com o Davi
- Quem são as 8 a 10 primeiras pessoas e quem conduz cada uma.
- Se ele topa usar este roteiro ou quer trocar perguntas.
- Quem preenche o mapa de concorrentes (sugestão: Gustavo pesquisa, Davi corrige pelo olhar técnico).
