# 🤖 Agente de IA para Validação e Gestão de Licitações de TI

> **Automação Inteligente End-to-End:** Da Triagem de Oportunidades (Go/No-Go) à Emissão da Ordem de Compra (Supply Chain) no Mercado de Vendas Públicas de Tecnologia.

---

## 📌 1. Contexto e Objetivos

### Contexto de Negócio
Vender equipamentos e soluções de Tecnologia da Informação (TI) para o setor público (Governo) exige extrema precisão técnica, fiscal e jurídica. O fluxo envolve a análise de editais complexos com dezenas de páginas, verificação rigorosa de requisitos técnicos de *hardware* (servidores, desktops, switches) e *software* (replicação de dados, backup), além do cumprimento de prazos curtos pós-disputa. Um único erro de interpretação no Termo de Referência (TR) pode resultar na inabilitação da empresa ou em prejuízos operacionais severos.

### Objetivos do Projeto
Este repositório documenta a arquitetura, engenharia de prompts e plano de validação de um **Agente de IA Especializado em Licitações de TI**. O objetivo principal é automatizar o fluxo de fornecimento em 5 módulos sequenciais, reduzindo o tempo de análise de editais, eliminando desclassificações técnicas e garantindo a transição perfeita entre a equipe comercial e a cadeia de suprimentos (*supply chain*).

---

## 📚 2. Curadoria de Fontes

Para fundamentar as regras de negócio, legislação e diretrizes operacionais do agente, foram selecionadas e integradas as seguintes fontes públicas oficiais:

1. **[Lei Federal nº 14.133/2021](https://www.in.gov.br/en/web/dou/-/lei-n-14.133-de-1-de-abril-de-2021-311876884)** — Nova Lei de Licitações e Contratos Administrativos (Regras de habilitação, prazos de impugnação, ritos e sanções).
2. **[Instrução Normativa SGD/ME nº 94/2022](https://www.gov.br/governodigital/pt-br)** — Diretrizes e regras para contratações de Soluções de Tecnologia da Informação e Comunicação (TIC) pelo Governo Federal.
3. **[Instrução Normativa SEGES/ME nº 65/2021](https://www.gov.br/compras/pt-br)** — Procedimentos para a realização de pesquisa de preços para aquisição de bens e contratação de serviços em geral.
4. **[Guia de Boas Práticas em Contratações de TI do TCU](https://portal.tcu.gov.br)** — Orientações do Tribunal de Contas da União para especificações técnicas, parcelamento do objeto e vedação ao direcionamento.
5. **[Modelos de Termo de Referência para TI da AGU](https://www.gov.br/agu/pt-br)** — Minutas padrão da Advocacia-Geral da União utilizadas pelos órgãos governamentais na contratação de bens de TIC.

---

## 🛠️ 3. Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Nesta seção estão registradas as estratégias de instrução adotadas, os desafios práticos enfrentados durante o desenvolvimento dos prompts e como foram solucionados.

### Estrutura dos Prompts
Os prompts foram projetados utilizando técnicas de **Few-Shot Prompting**, **Chain-of-Thought (Cadeia de Raciocínio)** e **Role-Playing**, atribuindo papéis especializados ao agente em cada módulo (Analista de Inteligência, Advogado Administrativo, Arquiteto de Soluções e Gerente de Operações).

### "Cicatrizes" do Processo (Troubleshooting & Lições Aprendidas)

| Desafio Encontrado (Cicatriz) | Causa Raiz | Solução Aplicada na Engenharia de Prompts |
| :--- | :--- | :--- |
| **1. Alucinação de Part Numbers e Especificações** | A IA tendia a assumir que o produto atendia ao edital sem verificar a ficha técnica real ou inventava códigos de fabricantes (*Part Numbers*). | **Trava de Conformidade Estrita:** Exigiu-se a classificação obrigatória em três categorias (`[ATENDE]`, `[ATENDE COM UPGRADE]` ou `[NÃO ATENDE]`), exigindo a citação direta do parâmetro no *datasheet*. |
| **2. Erro de Contagem de Prazos Legais** | Ambiguidade entre a contagem de dias úteis e dias corridos para impugnações e pedidos de esclarecimentos. | **Parametrização Explícita de Regras:** Inserção das regras explícitas dos arts. 164 e 165 da Lei 14.133/2021 diretamente no *System Prompt* do Módulo 2. |
| **3. Parsing de PDFs Complexos e Tabelas Desformatadas** | Editais com tabelas de especificações truncadas geravam saídas incompletas ou perda de requisitos. | **Formatação em Markdown com Fallback:** Instrução para reconstrução em tabelas Markdown estruturadas e emissão de alerta de "Requisito Não Identificado / Ambíguo". |

---

## 📖 4. Miniguia de Estudo (Entrega Final)

### 4.1. Resumo Estruturado dos 5 Módulos do Agente

[Entrada: Edital & TR] │ ▼ ┌─────────────────────────┐ │ 1. Triagem Go / No-Go   │ ──► [Filtro de Aderência, Atestados e Margem] └────────────┬────────────┘ │ (Aprovado) ▼ ┌─────────────────────────┐ │ 2. Estudo do Edital     │ ──► [Parsing de Prazos, Certidões e Impugnações] └────────────┬────────────┘ │ ▼ ┌─────────────────────────┐ │ 3. Matriz de Conformid. │ ──► [De-Para Técnico de Hardware/Software (BOM)] └────────────┬────────────┘ │ ▼ ┌─────────────────────────┐ │ 4. Obrigações Pós-Vitó. │ ──► [Proposta Readequada + Dossiê de Habilitação] └────────────┬────────────┘ │ ▼ ┌─────────────────────────┐ │ 5. Ordem de Compra (PO) │ ──► [Instruções de Aquisição & Supply Chain] └─────────────────────────┘

1. **Módulo 1 — Triagem Go/No-Go:** Avalia o objeto, acervo de atestados técnicos, viabilidade de prazos e margem financeira. Emite parecer `[GO]`, `[GO COM RESSALVAS]` ou `[NO-GO]`.
2. **Módulo 2 — Estudo do Edital:** Mapeia prazos críticos (impugnação, abertura, proposta), requisitos de habilitação e gera minutas de esclarecimento para a comissão.
3. **Módulo 3 — Matriz de Conformidade Técnica:** Realiza o "De-Para" item a item entre o TR e o *datasheet* do produto (servidores, redes, computadores), recomendando a melhor composição de hardware/software (BOM).
4. **Módulo 4 — Obrigações Pós-Vitória:** Recalcula a planilha de custos com o lance vencedor, monta a Proposta Comercial Readequada e compila o Dossiê de Habilitação.
5. **Módulo 5 — Instruções de Aquisição e Ordem de Compra:** Faz o *handover* para a equipe de suprimentos, emitindo a Ordem de Compra (PO) com Part Numbers (PNs) e cronograma logístico.

---

### 4.2. Glossário de Conceitos Fundamentais

* **TR (Termo de Referência):** Documento elaborado pelo órgão público que contém a especificação detalhada do objeto, requisitos contratuais, prazos e obrigações da contratada.
* **MAF (Manufacturer Authorization Letter):** Carta oficial de autorização do fabricante (ex: Dell, HP, Cisco) que garante o fornecimento, a originalidade e o suporte da garantia solicitada na licitação.
* **BOM (Bill of Materials):** Lista detalhada e codificada de todos os componentes (*Part Numbers*), licenças e pacotes de serviços que compõem a solução técnica.
* **Go / No-Go:** Metodologia de tomada de decisão rápida para filtrar oportunidades de negócio viáveis antes de alocar recursos operacionais.
* **RPO / RTO:** Indicadores de continuidade de negócios (*Recovery Point Objective* e *Recovery Time Objective*) que medem a perda tolerável de dados e o tempo de recuperação em soluções de replicação e backup.
* **EPEAT / Energy Star:** Certificações ambientais internacionais que atestam a eficiência energética e a sustentabilidade de equipamentos de hardware.
* **Proposta Readequada:** Proposta comercial ajustada ao valor final do lance vencedor da disputa, mantendo o equilíbrio físico-financeiro dos itens.
* **CNDT:** Certidão Negativa de Débitos Trabalhistas, documento obrigatório na fase de habilitação.

---

### 4.3. Prompts Reutilizáveis (Biblioteca Completa dos 5 Módulos)

#### 🔹 Prompt 1: Triagem Inicial Go/No-Go
```text
Você é um analista sênior de inteligência em licitações públicas de TI. Sua função é realizar a triagem prévia do Edital/TR anexado, comparando-o rigorosamente com nosso Portfólio de Produtos e Regras de Negócio.

Execute as seguintes checagens:
1. Identifique o objeto principal e confirme a aderência com nossas soluções (PCs, Servidores, Redes, Replicação).
2. Mapeie todas as exigências de Atestados de Capacidade Técnica e compare com nossa base.
3. Extraia o prazo de entrega estipulado e valide se é exequível.
4. Identifique cláusulas de alto risco, exigências de certificações ou prazos críticos.

Forneça o resultado em formato resumido, terminando obrigatoriamente com o status: [GO], [GO COM RESSALVAS] ou [NO-GO], acompanhado da justificativa em tópicos.
🔹 Prompt 2: Análise de Edital e Cronograma
Você é um especialista em direito administrativo e análise de editais de licitação de TI. Sua tarefa é analisar o Edital e o Termo de Referência fornecidos e realizar a extração completa das regras do certame.

Sua análise deve conter obrigatoriamente:
1. Cronograma cronológico (Esclarecimento, Impugnação, Abertura, Envio de Proposta/Amostras).
2. Requisitos de Habilitação (Jurídica, Fiscal, Financeira e Técnica) em formato de checklist.
3. Condições Comerciais e Operacionais (Prazo de entrega, SLA de garantia, penalidades e garantias contratuais).
4. Identificação de ambiguidades, falhas de especificação ou restrições indevidas, acompanhadas de uma minuta de pergunta/esclarecimento para a Comissão de Contratação.
🔹 Prompt 3: Matriz de Conformidade Técnica
Você é um Engenheiro de Soluções e Arquiteto de Pré-Vendas especializado em licitações de TI. Sua tarefa é cruzar os requisitos técnicos do Termo de Referência com a base de dados de produtos da empresa e gerar a Matriz de Conformidade Técnica.

Regras de validação:
1. Analise cada item exigido (Processador, Memória, Rede, Certificações, Softwares).
2. Selecione o produto do nosso portfólio que atenda 100% dos requisitos com o menor custo possível (evite superdimensionamento desnecessário).
3. Se o produto necessitar de um componente adicional para atender ao edital, especifique a necessidade de upgrade (ex: +1 fonte redundante).
4. Classifique cada requisito como: [ATENDE], [ATENDE COM UPGRADE] ou [NÃO ATENDE].
🔹 Prompt 4: Obrigações e Documentação Pós-Vitória
Você é um especialista em Operações Pós-Disputa e Compliance em Licitações Públicas. Sua tarefa é gerar a Proposta Readequada e organizar a Documentação de Habilitação do item vencido no certame.

Regras de execução:
1. Recalcule o valor unitário e total com base no lance final arrematado.
2. Preencha a proposta com as especificações técnicas exatas do produto selecionado no Módulo 3.
3. Verifique a validade de todas as certidões cadastradas no repositório da empresa.
4. Gere as declarações obrigatórias preenchidas com os dados da empresa e do certame.
5. Aponte qualquer pendência documental que necessite de atualização imediata antes do envio final.
🔹 Prompt 5: Instruções de Aquisição e Ordem de Compra (PO)
Você é um Gerente de Operações e Supply Chain especializado no atendimento de contratos públicos de TI. Sua tarefa é converter o contrato/nota de empenho homologado em Instruções de Aquisição e Ordem de Compra para nossa equipe de suprimentos.

Regras de execução:
1. Valide os dados da Nota de Empenho contra a nossa proposta final vencedora.
2. Monte a lista completa de materiais (BOM) detalhada com Part Numbers (PNs), licenças e pacotes de garantia.
3. Calcule a margem logística comparando o prazo do contrato com o SLA do distribuidor.
4. Gere a Ordem de Compra padronizada e as orientações de faturamento e entrega.
