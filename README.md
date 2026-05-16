# CNPJ Refactory — Agente para Migração ao Novo Formato Alfanumérico

Prompt de agente reutilizável para **auditar e migrar codebases** que precisam suportar o novo formato alfanumérico de CNPJ definido pela Instrução Normativa RFB nº 2.229/2024, com vigência a partir de julho de 2026.

## O Que o Agente Faz

A partir do código depositado na pasta `input/`, o agente executa quatro fases automaticamente:

1. **Varredura** — localiza todas as ocorrências relacionadas a CNPJ (regex, algoritmos, máscaras, testes, documentação)
2. **Classificação** — categoriza cada ocorrência por severidade (Crítico, Atenção, Informativo, Incerto)
3. **Relatório** — gera `outputs/cnpj-refactory-report.md` com o diagnóstico completo e código sugerido
4. **Aplicação** — opcionalmente aplica as correções diretamente nos arquivos (mediante confirmação)

## Estrutura de Pastas

```
refactory-cnpj/
├── agent.prompt.md   ← prompt do agente
├── README.md         ← este arquivo
├── input/            ← coloque aqui o código a ser analisado
└── outputs/          ← relatório gerado pelo agente (criado automaticamente)
```

## Como Usar

### 1. Pré-requisitos

- Um assistente de IA com suporte a **modo agente** (ex: VS Code com Copilot, Cursor, Continue, ou qualquer ferramenta compatível com `.prompt.md`)

### 2. Preparar o código

Copie os arquivos ou subpastas do projeto que deseja analisar para dentro de `input/`:

```
refactory-cnpj/
└── input/
    ├── src/
    ├── tests/
    └── composer.json   ← exemplo
```

O agente varre `input/` recursivamente, ignorando automaticamente `node_modules/`, `vendor/`, `bin/`, `dist/`, arquivos de lock e binários.

### 3. Invocar o agente

No seu assistente de IA (modo agente), escreva qualquer um dos gatilhos:

```
refatorar CNPJ
novo formato CNPJ
CNPJ alfanumérico
CNPJ 2026
```

Ou referencie o arquivo de prompt diretamente usando `#agent.prompt.md` (ou o mecanismo equivalente da sua ferramenta).

### 4. Revisar o relatório

O agente gera `outputs/cnpj-refactory-report.md` com:

- Resumo executivo por severidade
- Cada ocorrência com código atual, código sugerido e justificativa
- Checklist de validação pós-refatoração
- Apêndice com cálculos de CNPJs de teste

### 5. Aplicar as alterações (opcional)

Ao final, o agente pergunta:

```
Deseja que eu aplique as alterações sugeridas diretamente nos arquivos originais?

  [S] Sim — aplicar todas as alterações Críticas e Atenção agora
  [P] Parcial — arquivo por arquivo para confirmar cada um
  [N] Não — manter apenas a documentação
```

Responda com **S**, **P** ou **N**.

## Severidades

| Ícone | Severidade | Significado |
|---|---|---|
| 🔴 | Crítico | Quebrará CNPJs alfanuméricos em produção |
| 🟡 | Atenção | Pode quebrar dependendo do contexto |
| 🟢 | Informativo | Comentários e documentação — sem impacto funcional |
| ⚪ | Incerto | Não foi possível determinar com segurança; documentado mas não alterado |

## O Que o Agente Verifica

| Categoria | Exemplos |
|---|---|
| Regex / Validação | `\d{14}`, `\d{2}\.\d{3}`, funções `validarCNPJ` |
| Algoritmo módulo 11 | Cálculo de dígitos verificadores com caracteres alfanuméricos |
| Máscaras de input | `v-mask`, `imask`, `ngx-mask`, atributo `pattern` |
| Formatação / Display | `formatCnpj`, `maskCnpj`, pipes Angular/Vue |
| Banco de dados | Colunas `INT`, `BIGINT`, `CHAR` de tamanho insuficiente |
| Testes | Fixtures e mocks com CNPJs somente numéricos |
| Documentação | Comentários, exemplos Swagger, `.env.example` |

## O Novo Formato em Resumo

| | Formato atual | Novo formato |
|---|---|---|
| **Tipo** | Somente numérico | Alfanumérico (A–Z e 0–9) |
| **Raiz + ordem** (posições 1–8) | `\d{8}` | `[A-Z0-9]{8}` |
| **Filial** (posições 9–12) | `\d{4}` | `\d{4}` (somente dígitos) |
| **Dígitos verificadores** (13–14) | `\d{2}` | `\d{2}` (somente dígitos) |
| **Exemplo** | `12.345.678/0001-95` | `A1.B2C.D3E/0001-88` |
| **Retrocompatibilidade** | — | CNPJs numéricos continuam válidos |

## Metadados do Prompt

| Campo | Valor |
|---|---|
| **Nome** | `CNPJ Refactory` |
| **Arquivo** | `agent.prompt.md` |
| **Ferramentas** | `read`, `edit`, `search`, `create`, `todo` |
| **Dica de argumento** | Coloque seu código na pasta `input/` e invoque o agente |
| **Referência normativa** | Instrução Normativa RFB nº 2.229/2024 |
| **Vigência** | Julho de 2026 |
