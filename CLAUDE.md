# SIEM-IA (nome de trabalho)

SIEM para PME com triagem por IA: classificador de ML dá a probabilidade, LLM explica em português, alerta chega no celular. Sociedade entre Gustavo (IA e software) e Davi (redes e cyber). Plano completo em `docs/PLANO.md`; decisões em `docs/DECISOES.md`.

## Regras do projeto
- Duas pessoas, cada uma com seu Claude Code, um repositório. Antes de trabalhar: `git pull`. Nunca commitar direto na `main`: branch curta + Pull Request revisado pelo outro.
- Commits e código em inglês; documentos em português do Brasil.
- Decisão de arquitetura, produto ou negócio vai para `docs/DECISOES.md` com data e motivo. Não deixar só na conversa.
- A probabilidade de um alerta vem sempre do classificador de ML, nunca do LLM. O LLM só explica e deve citar as linhas do log que sustentam a análise.
- Ação sugerida pelo sistema não é executada sozinha; qualquer automação de resposta exige aprovação humana.

## Segurança (obrigatório)
- Nunca commitar log real de cliente, dado pessoal, chave de API, token ou `.env`. Dados de teste são sempre do laboratório ou sintéticos.
- Não enviar log de cliente real para API externa de LLM sem contrato de dados que cubra isso.
- Segredos só em variáveis de ambiente.

## Comandos de build/teste
Ainda não há código. Quando houver, registrar aqui os comandos de teste, lint e execução local.
