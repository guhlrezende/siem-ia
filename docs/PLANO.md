# Plano do projeto (nome de trabalho: SIEM-IA)

Versão 0.1 · 2026-11-05 · Autores: Gustavo e Davi · Status: rascunho para os dois revisarem

Tudo aqui é hipótese até ser validado. O que não tem fonte ou teste está marcado como **a validar**.

## 1. Ideia em uma frase

Um SIEM para pequenas e médias empresas que não entrega só o log: entrega o alerta já analisado, com a probabilidade de qual é o problema, uma explicação em português e a ação sugerida, direto no celular de quem decide.

## 2. Problema e cliente

- **Problema:** SIEM tradicional despeja log e exige um analista para interpretar. PME quase nunca tem analista nem SOC. O dono ou um único profissional de TI recebe o ruído, ou não recebe nada.
- **Cliente-alvo (hipótese):** empresa de 10 a 200 funcionários, com servidores próprios ou na nuvem, sem equipe de segurança dedicada. Quem recebe o alerta é o dono, o gestor ou o TI de plantão.
- **Dor a validar:** "se eu fosse atacado hoje, eu ficaria sabendo na hora?". A venda é tranquilidade e resposta rápida, não tecnologia.
- **Concorrência a mapear (Fase 0):** serviços gerenciados de Wazuh, MDR para PME, módulos de IA dos SIEMs grandes (Microsoft Sentinel, Google SecOps), antivírus/EDR com alerta no celular. Preencher preço, público e o que cada um não faz.
- **Diferencial proposto:** preço de PME, português, zero analista, alerta acionável no celular com aprovação de ação em um toque.

## 3. Arquitetura proposta

```
Máquinas do cliente (agente Wazuh)
        |
        v
Coletor/correlação (Wazuh Manager)
        |
        v
Backend na nuvem (multi-cliente, isolado por tenant)
   1. Normalização do evento
   2. Filtro e classificação (ML): tipo de ameaça + probabilidade
   3. Agrupamento (evita alerta repetido)
   4. LLM: explicação em português + ação sugerida + linhas do log como prova
        |
        v
Push (Firebase Cloud Messaging) --> App móvel: pop-up, detalhes, botão de ação
```

Regras de desenho:
- **A probabilidade vem do classificador, nunca do LLM.** O LLM não é calibrado. Ele explica o que o classificador decidiu.
- **O LLM só vê o que passou do filtro.** Controla custo e latência.
- **Toda explicação cita as linhas do log que a sustentam.** Reduz alucinação e deixa o Davi auditar.
- **Ação sugerida não é ação executada** no começo. Executar (bloquear IP, desativar conta) só depois de a Fase 5 provar que o alerta é confiável, e sempre com aprovação humana.

## 4. Decisões técnicas em aberto

| Decisão | Opções | Quem decide |
|---|---|---|
| Coletor base | Wazuh (open source) vs outro | Davi |
| Modelo de classificação | Random Forest/XGBoost primeiro; rede neural só se os dados justificarem | Gustavo |
| LLM | API paga vs modelo local (Ollama). Custo pesa porque o orçamento é baixo | Gustavo |
| Backend | Python (FastAPI) + Postgres, a confirmar | Gustavo |
| App | Flutter vs React Native | Gustavo |
| Hospedagem | A definir pelo custo (nuvem barata, ou VPS) | Os dois |
| Nome do produto | A definir (checar registro no INPI e domínio) | Os dois |

Cada decisão fechada vai para `docs/DECISOES.md` com data e motivo.

## 5. Roteiro por fases

Cada fase só termina quando o critério de pronto é cumprido e mostrado, não quando "parece ok".

**Fase 0. Validar o mercado (antes de gastar semanas codando)**
- Conversar com 8 a 10 donos ou TIs de PME (o Davi provavelmente conhece vários). Perguntar o que usam hoje, quanto pagam, se já tiveram incidente, se pagariam e quanto.
- Mapear 5 concorrentes (preço, público, limite).
- Pronto quando: pelo menos 3 pessoas dizem que pagariam algo (valor anotado) e a dor principal está confirmada ou corrigida.

**Fase 1. Laboratório**
- Wazuh rodando com 2 ou 3 máquinas de teste e ataques simulados (força bruta SSH/RDP, login de local incomum, comportamento tipo ransomware).
- Pronto quando: o Davi consegue disparar cada ataque e ver o log chegando ao backend.

**Fase 2. Classificador**
- Dataset: logs do próprio laboratório rotulados pelo Davi, mais datasets públicos (CICIDS2017, UNSW-NB15, Boss of the SOC) **a validar quanto à licença e à adequação a logs do Wazuh**.
- Baseline simples primeiro. Medir precisão, recall e taxa de falso positivo por dia.
- Pronto quando: meta inicial de falso positivo e recall definida pelos dois e atingida no laboratório. Os números da meta saem do Davi, que sabe o que um TI tolera.

**Fase 3. Camada de LLM**
- Explicação em português, ação sugerida, mapeamento MITRE ATT&CK, linhas do log citadas.
- Pronto quando: o Davi avalia 50 alertas explicados às cegas e aprova a maioria (critério a combinar). Registrar os erros.

**Fase 4. App móvel**
- Push, tela do alerta, histórico, botão de ação (ainda só registra a decisão), login seguro.
- Pronto quando: um ataque simulado gera pop-up no celular em menos de um tempo-alvo (definir, por exemplo 1 minuto) com a explicação completa.

**Fase 5. Piloto**
- 1 ou 2 empresas reais, com contrato de piloto e termo de dados (ver seção 6). Medir falso positivo real, alertas ignorados, alertas úteis.
- Pronto quando: o piloto pelo menos mantém o app instalado e a leitura dos alertas por 30 dias, e dá um depoimento ou um "pagaria".

**Fase 6. Comercial**
- Preço, contrato, cobrança, suporte, página de venda.

## 6. O que precisa existir para virar produto vendável

**Jurídico e dados**
- **LGPD:** log tem dado pessoal (IP, usuário, e-mail). Vocês seriam operadores de dados do cliente. Precisa de contrato com cláusula de tratamento de dados, finalidade, retenção e exclusão. **Consultar advogado antes do piloto.**
- **Responsabilidade:** o contrato deve deixar claro que o produto é apoio à decisão, sem garantia de detecção. Falha em avisar um ataque real é o maior risco jurídico e de reputação.
- **Sociedade entre Gustavo e Davi:** divisão, quem entra com o quê, o que acontece se um sair, propriedade do código. Conversar agora e registrar por escrito (acordo de sócios). Para vender de verdade, formalizar uma empresa (CNPJ), com contador.
- **Marca:** checar o nome no INPI e o domínio antes de investir em identidade.

**Segurança do próprio produto**
- Um SIEM guarda os logs de vários clientes, então é alvo. Exige isolamento entre clientes, criptografia em trânsito e em repouso, segredos fora do código, autenticação forte no app, registro de acesso.
- Nunca enviar dado real de cliente para API de LLM sem o contrato cobrir isso, ou usar modelo local para esses dados.

**Produto**
- Onboarding simples: instalar o agente em poucos passos, sem o cliente precisar entender Wazuh.
- Controle de fadiga de alerta: só alta confiança vira push; o resto vai em resumo diário.
- Monitoramento do próprio sistema (se o coletor cair, o cliente precisa saber).
- Suporte: quem atende, em que horário, em quanto tempo. PME espera resposta humana.

**Negócio**
- Modelo: assinatura mensal por máquina ou por empresa (**a validar na Fase 0**).
- Custo por cliente: infraestrutura + LLM + suporte. Calcular antes de fixar preço.
- Aquisição: começar pela rede do Davi, depois indicações, depois contadores e consultorias de TI que atendem várias PMEs (canal de revenda).
- Prova social: o piloto vira estudo de caso.

## 7. Riscos principais

| Risco | Mitigação |
|---|---|
| Falso positivo demais: cliente desliga as notificações | Meta de falso positivo na Fase 2, push só para alta confiança, resumo diário |
| Falso negativo: ataque real passa | Contrato claro, cobertura de poucos casos bem feitos, nunca prometer proteção total |
| LLM alucina | Probabilidade vem do classificador, citação de log obrigatória, revisão do Davi |
| Custo de LLM e infra maior que o preço | Filtro antes do LLM, modelo local, cálculo de custo por cliente |
| Vazamento de dado de cliente | Isolamento por tenant, criptografia, contrato de dados |
| Escopo grande demais | MVP com 3 casos de ataque, uma fonte de log (Wazuh) |
| Desalinhamento entre os dois | Repositório único, `DECISOES.md`, PRs pequenos, reunião curta semanal |
| Concorrente gratuito ou grande faz o mesmo | Foco em PME brasileira, preço, suporte em português |

## 8. Papéis

- **Davi (redes e cyber):** laboratório, fontes de log, simulação de ataques, rotulagem, validação técnica de cada alerta, conversa com clientes de TI.
- **Gustavo (IA e software):** classificador, camada de LLM, backend, app, infraestrutura de código.
- **Os dois:** Fase 0, decisões de produto, preço, contrato, sociedade.

A divisão é uma proposta. Ajustem conforme o que cada um quer fazer.

## 9. Como trabalhamos (Git)

- Branch `main` protegida na prática: ninguém commita direto, tudo entra por Pull Request revisado pelo outro.
- Branches curtas por tarefa: `feat/...`, `fix/...`, `docs/...`, `lab/...`.
- Commits em inglês, mensagem descritiva. Documentos em português.
- Decisão tomada vai para `docs/DECISOES.md`, não fica só no chat.
- Antes de começar uma sessão: `git pull`. Antes de encerrar: commit e push do que estiver pronto.
- Dado real de cliente, log de produção, chave de API, `.env`: nunca no repositório (ver `.gitignore`).

## 10. Próximos passos imediatos

1. Davi e Gustavo leem este plano e marcam o que discordam.
2. Fechar o nome de trabalho e criar o repositório privado no GitHub.
3. Preparar o roteiro de entrevista da Fase 0 e escolher as primeiras 8 a 10 pessoas.
4. Conversar sobre sociedade e divisão (seção 6) antes de investir tempo em código.
5. Davi sobe o laboratório da Fase 1 enquanto a Fase 0 acontece.

## 11. Perguntas abertas

- Qual é a disponibilidade de tempo de cada um por semana?
- Há orçamento mensal para infraestrutura e LLM? De quanto?
- O Davi já tem contatos de PMEs para a Fase 0?
- Windows, Linux ou os dois como alvo do MVP?
- O produto roda na nuvem de vocês ou parte do coletor fica no cliente?
