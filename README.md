# 📊 CryptDecision (SAD)

> **Plataforma Web para Alocação Estratégica de Capital Corporativo baseada em Análise Decisória Multicritério, Segurança Multitenant (RLS) e Auditabilidade Criptográfica (SHA-256).**

![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla%20JS-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=for-the-badge&logo=supabase&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chartdotjs&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

---

## 🎯 Sobre o Projeto

O **DecisionSupport** é um **Sistema de Apoio à Decisão (SAD / DSS)** projetado para resolver o problema clássico de alocação de recursos financeiros em ambientes corporativos com orçamentos restritos.

Diferentemente de abordagens baseadas em Aprendizado de Máquina opaco (*caixa-preta*), o sistema emprega um **algoritmo determinístico e transparente de Análise Decisória Multicritério**. Cada proposta orçamentária é avaliada sob 4 indicadores fundamentais (**Necessidade, Urgência, Impacto e Retorno Esperado**), gerando um *ranking* escalar e executando o rateio proporcional do capital disponível sob restrições estritas de **piso mínimo e teto máximo**.

> 🎓 **Projeto de Conclusão de Curso (TCC)**  
> **Instituição:** Universidade Paulista (UNIP) — 2026  
> **Curso:** Ciência da Computação / Sistemas de Informação  

---

## 🚀 Funcionalidades Principais

- 🏢 **Isolamento Multitenant (Múltiplas Empresas):** Suporte à gestão de diferentes organizações sob a mesma conta, com seleção dinâmica no Dashboard.
- 🧮 **Motor de Cálculo Multicritério (`businessRules.js`):**
  - Normalização *Min-Max Scaling* de indicadores na escala 0–100.
  - Média ponderada com pesos customizáveis pela governança corporativa.
  - Distribuição proporcional de capital com **ajuste fino de arredondamento a R$ 0,00 de sobra (100,00% alocado)**.
  - Trava de teto máximo por setor para impedir a concentração predatória de recursos.
- 💰 **Gestão Financeira Integrada:** Controle de caixa (Receitas, Despesas e Investimentos) com cálculo de saldo acumulado no tempo.
- 📊 **Dashboards Interativos em Canvas:** Indicadores em tempo real e gráficos interativos (*Chart.js*) para diagnóstico de solvência e evolução mensal.
- 🔒 **Defesa em Profundidade & Segurança no Banco:**
  - Autenticação *stateless* por tokens *JWT* via *Supabase Auth*.
  - Políticas de *Row Level Security (RLS)* no *PostgreSQL* para garantir o isolamento rígido de dados no motor do banco.
- 📜 **Auditabilidade Criptográfica (SHA-256):**
  - Módulo nativo no cliente (`security.js` via *Web Crypto API*) que gera uma assinatura digital *SHA-256* de 64 caracteres em cada simulação para garantir o não-repúdio dos pareceres.
- 📄 **Exportação de Relatórios Executivos:**
  - **PDF Corporativo (3 Páginas):** Capa no padrão *McKinsey*, sumário analítico e termos de homologação.
  - **Planilha Executiva (.XLS):** Exportador em tabela de 6 colunas delimitadas com marcadores *BOM UTF-8*, cores por nível de prioridade e rodapé de auditoria.

---

## 🛠️ Stack Tecnológica

### **Frontend & Arquitetura Web**
- **Vanilla JavaScript (ES6+):** Código nativo puro sem a sobrecarga de *frameworks* pesados, garantindo controle direto da memória e manipulação do DOM.
- **Vite:** Ferramenta de compilação e empacotamento ágil para *Single Page Applications (SPA)*.
- **Chart.js:** Renderização gráfica de alta performance sobre *Canvas HTML5*.
- **jsPDF & jspdf-autotable:** Compilação e diagramação de relatórios *PDF* no lado do cliente.

### **Backend, Banco de Dados & Segurança**
- **Supabase (BaaS):** Plataforma de persistência e autenticação em nuvem.
- **PostgreSQL:** Banco de dados relacional normalizado na **3FN**.
- **Row Level Security (RLS):** Segurança declarativa em nível de linha no *PostgreSQL* acionada por `auth.uid()`.
- **Web Crypto API (`crypto.subtle`):** Algoritmo *SHA-256* nativo para assinatura digital de relatórios.

---

## 🏗️ Arquitetura do Sistema


[ Cliente (Navegador SPA) ]
       │
       ├── Router (router.js) ─────────> Controle de Rotas e Route Guards
       ├── API Facade (api.js) ────────> Chamadas Assíncronas (Fetch / HTTPS)
       ├── Motor SAD (businessRules.js) ──> Normalização Min-Max + Média Ponderada
       └── Criptografia (security.js) ─> Geração de Hash SHA-256 no Cliente
       │
       ▼ (Cabeçalho HTTP + Bearer Token JWT)
[ Supabase BaaS ]
       │
       └── PostgreSQL Engine ──────────> Filtro RLS Nativo (owner_id = auth.uid())
       ---

## 🗄️ Modelo do Banco de Dados (DER)

A estrutura relacional foi projetada no **PostgreSQL** e hospedada no **Supabase**, garantindo integridade referencial e normalização em **3FN**:

- **`companies`**: Cadastro institucional das empresas e vínculo com o proprietário (`owner_id`).
- **`areas`**: Departamentos/setores cadastrados para cada organização.
- **`financial_transactions`**: Histórico de receitas (+), despesas (-) e investimentos (+).
- **`indicators`**: Pontuações de 0 a 100 (*Necessidade, Urgência, Impacto e Retorno*).
- **`business_rules`**: Pesos estratégicos e travas de teto por empresa.
- **`analyses` & `analysis_results`**: Histórico de simulações, notas calculadas e verbas alocadas.

