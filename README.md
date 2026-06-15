# ERP Roadmap em PHP e Laravel

## Objetivo

Este repositório contém uma trilha de projetos para aprender desenvolvimento de sistemas ERP utilizando PHP puro e Laravel.

A proposta é evoluir gradualmente desde um simples CRUD até um ERP SaaS completo com múltiplos módulos empresariais.

---

# Nível 1 — Fundamentos

## Projeto 1 — Cadastro Comercial

### Objetivo

Criar um sistema para gerenciar clientes e produtos.

### Funcionalidades

* Cadastro de clientes
* Cadastro de produtos
* Edição
* Exclusão
* Pesquisa

### Fluxo

```text
Administrador
    ↓
Cadastrar Cliente
    ↓
Cadastrar Produto
    ↓
Consultar Dados
```

### Estrutura SQL

```sql
CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150),
    email VARCHAR(150),
    telefone VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE produtos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150),
    descricao TEXT,
    preco DECIMAL(10,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# Projeto 2 — Controle de Estoque

### Objetivo

Controlar entrada e saída de produtos.

### Funcionalidades

* Entrada de estoque
* Saída de estoque
* Histórico de movimentações
* Saldo atual

### Fluxo

```text
Compra Produto
    ↓
Entrada Estoque
    ↓
Venda Produto
    ↓
Saída Estoque
```

### Estrutura SQL

```sql
CREATE TABLE estoque_movimentacoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    produto_id INT,
    tipo ENUM('entrada','saida'),
    quantidade INT,
    observacao TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (produto_id)
    REFERENCES produtos(id)
);
```

---

# Projeto 3 — Sistema de Vendas

### Objetivo

Registrar pedidos e vendas.

### Funcionalidades

* Carrinho
* Pedidos
* Itens do pedido
* Controle de status

### Fluxo

```text
Cliente
    ↓
Pedido
    ↓
Itens
    ↓
Pagamento
```

### Estrutura SQL

```sql
CREATE TABLE pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    valor_total DECIMAL(10,2),
    status ENUM('aberto','pago','cancelado'),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY(cliente_id)
    REFERENCES clientes(id)
);

CREATE TABLE pedido_itens (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id INT,
    produto_id INT,
    quantidade INT,
    valor_unitario DECIMAL(10,2),

    FOREIGN KEY(pedido_id)
    REFERENCES pedidos(id),

    FOREIGN KEY(produto_id)
    REFERENCES produtos(id)
);
```

---

# Projeto 4 — Financeiro

### Objetivo

Controlar contas a pagar e receber.

### Funcionalidades

* Fluxo de caixa
* Contas a pagar
* Contas a receber
* Relatórios

### Estrutura SQL

```sql
CREATE TABLE contas_receber (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    valor DECIMAL(10,2),
    vencimento DATE,
    status ENUM('pendente','pago'),

    FOREIGN KEY(cliente_id)
    REFERENCES clientes(id)
);

CREATE TABLE contas_pagar (
    id INT AUTO_INCREMENT PRIMARY KEY,
    descricao VARCHAR(255),
    valor DECIMAL(10,2),
    vencimento DATE,
    status ENUM('pendente','pago')
);

CREATE TABLE caixa (
    id INT AUTO_INCREMENT PRIMARY KEY,
    tipo ENUM('entrada','saida'),
    valor DECIMAL(10,2),
    descricao VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# Nível 2 — Mini ERP Comercial

## Projeto 5 — ERP Comercial Básico

### Objetivo

Integrar vendas, estoque e financeiro.

### Fluxo

```text
Venda
 ↓
Pedido
 ↓
Saída Estoque
 ↓
Conta Receber
 ↓
Movimento Caixa
```

### Módulos

* Clientes
* Produtos
* Estoque
* Vendas
* Financeiro

---

# Nível 3 — Laravel

## Projeto 6 — Oficina Mecânica

### Objetivo

Gerenciar veículos e ordens de serviço.

### Fluxo

```text
Cliente
 ↓
Veículo
 ↓
Ordem de Serviço
 ↓
Peças
 ↓
Pagamento
```

### SQL

```sql
CREATE TABLE veiculos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    marca VARCHAR(100),
    modelo VARCHAR(100),
    placa VARCHAR(20),

    FOREIGN KEY(cliente_id)
    REFERENCES clientes(id)
);

CREATE TABLE ordens_servico (
    id INT AUTO_INCREMENT PRIMARY KEY,
    veiculo_id INT,
    descricao TEXT,
    status ENUM(
        'aberta',
        'analise',
        'concluida'
    ),
    valor_total DECIMAL(10,2),

    FOREIGN KEY(veiculo_id)
    REFERENCES veiculos(id)
);
```

---

## Projeto 7 — Assistência Técnica

### Fluxo

```text
Recebimento
 ↓
Diagnóstico
 ↓
Orçamento
 ↓
Aprovação
 ↓
Reparo
```

### SQL

```sql
CREATE TABLE equipamentos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    equipamento VARCHAR(150),
    marca VARCHAR(100),
    modelo VARCHAR(100),

    FOREIGN KEY(cliente_id)
    REFERENCES clientes(id)
);

CREATE TABLE chamados (
    id INT AUTO_INCREMENT PRIMARY KEY,
    equipamento_id INT,
    defeito TEXT,
    status ENUM(
        'recebido',
        'analise',
        'aguardando',
        'concluido'
    ),

    FOREIGN KEY(equipamento_id)
    REFERENCES equipamentos(id)
);
```

---

## Projeto 8 — Compras

### Fluxo

```text
Fornecedor
 ↓
Pedido de Compra
 ↓
Recebimento
 ↓
Entrada Estoque
```

### SQL

```sql
CREATE TABLE fornecedores (
    id INT AUTO_INCREMENT PRIMARY KEY,
    razao_social VARCHAR(255),
    cnpj VARCHAR(20),
    telefone VARCHAR(20)
);

CREATE TABLE compras (
    id INT AUTO_INCREMENT PRIMARY KEY,
    fornecedor_id INT,
    valor_total DECIMAL(10,2),
    status ENUM('aberta','recebida'),

    FOREIGN KEY(fornecedor_id)
    REFERENCES fornecedores(id)
);
```

---

# Nível 4 — ERP Completo

## Projeto 9 — Controle de Usuários e Permissões

### SQL

```sql
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150),
    email VARCHAR(150),
    senha VARCHAR(255)
);

CREATE TABLE perfis (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100)
);

CREATE TABLE permissoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150)
);

CREATE TABLE perfil_permissao (
    perfil_id INT,
    permissao_id INT
);
```

---

## Projeto 10 — CRM

### Fluxo

```text
Lead
 ↓
Contato
 ↓
Proposta
 ↓
Cliente
```

### SQL

```sql
CREATE TABLE leads (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150),
    email VARCHAR(150),
    telefone VARCHAR(20),
    origem VARCHAR(100),
    status VARCHAR(50)
);

CREATE TABLE propostas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    lead_id INT,
    valor DECIMAL(10,2),
    status VARCHAR(50),

    FOREIGN KEY(lead_id)
    REFERENCES leads(id)
);
```

---

# Nível 5 — SaaS

## Projeto 11 — Multiempresa

### Objetivo

Permitir múltiplas empresas usando o mesmo sistema.

### Fluxo

```text
Empresa A
Empresa B
Empresa C
```

Cada empresa visualiza apenas seus próprios dados.

### SQL

```sql
CREATE TABLE empresas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255),
    cnpj VARCHAR(20),
    plano VARCHAR(50)
);
```

Adicionar:

```sql
empresa_id INT
```

nas tabelas:

* clientes
* produtos
* pedidos
* compras
* caixa
* estoque

---

## Projeto 12 — Produção (MRP)

### Fluxo

```text
Produto Final
 ↓
Consome Matéria-prima
 ↓
Produção
 ↓
Estoque
```

### SQL

```sql
CREATE TABLE estrutura_produto (
    id INT AUTO_INCREMENT PRIMARY KEY,
    produto_final INT,
    materia_prima INT,
    quantidade DECIMAL(10,2)
);

CREATE TABLE ordem_producao (
    id INT AUTO_INCREMENT PRIMARY KEY,
    produto_id INT,
    quantidade INT,
    status ENUM(
        'aberta',
        'producao',
        'finalizada'
    )
);
```

---

## Projeto 13 — RH

### Fluxo

```text
Funcionário
 ↓
Cargo
 ↓
Folha
 ↓
Férias
```

### SQL

```sql
CREATE TABLE funcionarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150),
    cpf VARCHAR(14),
    salario DECIMAL(10,2)
);

CREATE TABLE folha_pagamento (
    id INT AUTO_INCREMENT PRIMARY KEY,
    funcionario_id INT,
    referencia DATE,
    salario DECIMAL(10,2),

    FOREIGN KEY(funcionario_id)
    REFERENCES funcionarios(id)
);
```

---

# Projeto Final — ERP SaaS Empresarial

## Stack Recomendada

* PHP 8+
* Laravel 12+
* MySQL
* Redis
* Docker
* Queue Workers
* WebSockets
* API REST
* JWT ou Sanctum

## Módulos

### CRM

* Leads
* Propostas
* Clientes

### Comercial

* Pedidos
* Orçamentos

### Estoque

* Entradas
* Saídas
* Inventário

### Compras

* Fornecedores
* Pedidos de compra

### Financeiro

* Caixa
* Bancos
* Contas a pagar
* Contas a receber

### Produção

* MRP
* Ordem de Produção

### RH

* Funcionários
* Folha

### Fiscal

* NF-e
* NFC-e

### BI

* Dashboards
* Indicadores
* Relatórios

---

# Roadmap de Estudo

## PHP Puro

1. Cadastro Comercial
2. Estoque
3. Vendas
4. Financeiro
5. Mini ERP Integrado

## Laravel

6. Oficina Mecânica
7. Assistência Técnica
8. Compras
9. CRM
10. ERP Comercial

## Avançado

11. Multiempresa
12. Produção (MRP)
13. RH
14. Fiscal
15. ERP SaaS Completo

---

# Resultado Esperado

Ao concluir todos os projetos você terá experiência prática em:

* Modelagem de banco de dados
* Regras de negócio empresariais
* PHP Orientado a Objetos
* Laravel
* APIs REST
* Autenticação
* Permissões
* Multi-tenancy
* ERP Comercial
* ERP Industrial
* SaaS Empresarial
* Arquitetura escalável
