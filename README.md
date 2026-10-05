# Agente Gatekeeper de GCM - Validador de Commits com IA

## Sobre o Projeto
Este repositório faz parte da avaliação da disciplina de Gestão de Configuração e Mudanças (GCM)[cite: 1]. O objetivo é implementar um Agente Autônomo de Inteligência Artificial, intermediado pela ferramenta de automação n8n, para atuar como um revisor automático de código (Gatekeeper)[cite: 1].

O agente é acionado a cada push, analisa o diff do commit focado numa única função de negócio e emite um veredito de conformidade em tempo real para evitar falhas em produção[cite: 1].

## Regra de Negócio Validada
Para garantir o foco prático na governança de mudanças, este projeto valida estritamente uma única função e a sua respetiva regra[cite: 1]:

- Função Alvo: calcularDesconto (preco, categoria)[cite: 1]
- Regra Documentada: O desconto máximo permitido aplicado diretamente no código é de 20% (0.20)[cite: 1]. Valores superiores exigem aprovação gerencial externa[cite: 1].
- Vereditos da IA:
  - APROVADO: Quando a modificação mantém o desconto em 20% (0.20) ou menos[cite: 1].
  - REPROVADO: Quando a modificação ultrapassa o limite estipulado (ex: 0.35), apontando a quebra da regra[cite: 1].

## Tecnologias Utilizadas
- GitHub (Webhooks): Para configurar eventos de push apontando para o n8n[cite: 1].
- GitHub REST API: Para extração do diff puro através da configuração do cabeçalho Accept com application/vnd.github.v3.diff[cite: 2].
- n8n: Ferramenta orquestradora para o fluxo de automação[cite: 1].
- Agente de IA (LLM): Injetado com a regra de negócio no prompt de sistema para emitir o veredito[cite: 1].

## Arquitetura do Fluxo
1. O programador realiza um commit e um push com alterações na função de desconto.
2. O repositório dispara o evento via Webhook para o n8n[cite: 1].
3. O n8n utiliza um nó HTTP Request na API de commits do GitHub para extrair o diff da alteração[cite: 1].
4. O texto do diff é analisado pelo Agente de IA de acordo com as restrições definidas no prompt do sistema[cite: 1, 2].
5. O agente emite a resposta final, aprovando commits válidos ou rejeitando inválidos com justificação[cite: 2].
