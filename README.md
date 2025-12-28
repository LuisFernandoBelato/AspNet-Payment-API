## 🌐 Language | Idioma

[🇺🇸 English (Default)](#en) | [🇧🇷 Português](#pt)

<div id="en"></div>

---

# Payment API Gateway \ ***6th Term - LP1***

This project implements a fictitious Payment API Gateway, developed in ASP.NET Core 8.0, to manage basic card operations and financial transactions.

## Overview

The API functions as an entry point for payment operations, offering endpoints for card validation, installment calculation, transaction registration, and status inquiry/modification (confirm or cancel).

## Main Features

*   **Card Validation:** Checks the validity of the card number in a database and identifies the brand (Visa, Mastercard, Elo) based on a simplified pattern.
*   **Installment Calculation:** Calculates the value of the installments of a transaction based on the total amount, interest rate, and number of installments.
*   **Payment Processing:**
    *   Registers a new transaction in the database with "PENDING" status.
    *   Checks the validity of the card before registering the transaction.
*   **Transaction Status Inquiry:** Returns the current status of a transaction (PENDING, CONFIRMED, CANCELED).
*   **Payment Confirmation:** Changes the status of a transaction from "PENDING" to "CONFIRMED", with validations to prevent confirmations of already canceled transactions.
*   **Payment Cancellation:** Changes the status of a transaction from "PENDING" to "CANCELED", with validations to prevent cancellations of already confirmed transactions.

## Technologies Used

*   **Database:** MySQL (`MySql.Data` for connection)
*   **Authentication:** JWT Bearer (configured via `Microsoft.AspNetCore.Authentication.JwtBearer`)
*   **Logging:** Serilog (`Serilog.AspNetCore`, `Serilog.Formatting.Compact`, `Serilog.Sinks.File`, `Serilog.Sinks.MySQL`, `Serilog.Sinks.MariaDB`) for event and error logging.
*   **API Documentation:** Swagger/OpenAPI (`Swashbuckle.AspNetCore`)

## Domain Models

*   **`Cartao`**: Represents a card with `Numero` (Number) and `Validade` (Validity).
*   **`Pagamento`**: Represents a payment transaction, including `_ValorTotal`, `_TaxaDeJuros`, `_QtdeParcelas`, `_Cartao`, `_CVV`, and `_Situacao` (an `enum`).
*   **`Situacao`**: Enumeration for transaction statuses: `PENDENTE (1)`, `CONFIRMADO (2)`, `CANCELADO (3)`.
*   **`Parcela`**: Represents a payment installment, with `_Parcela` (installment number) and `_Valor` (value).
  
### Services (`CartaoService.cs`, `PagamentoService.cs`)

*   **`CartaoService`**: Contains the logic for `ValidarCartao`, which checks the existence of the card number and its validity in the `Cartao` table of the database.
*   **`PagamentoService`**: Contains the business logic for:
    *   `CalcularParcelas`: Performs simple interest calculation for the installments.
    *   `GravarPagamento`: Inserts a new transaction into the `Transacao` table.
    *   `ConsultaSituacaoPagamento`: Queries the status of a transaction by ID.
    *   `ConfirmarPagamento`: Updates the transaction status to `CONFIRMED`, with state validation.
    *   `CancelarPagamento`: Updates the transaction status to `CANCELED`, with state validation.

### Controllers (`CartoesController.cs`, `PagamentosController.cs`)

These controllers expose the API endpoints, receive HTTP requests, delegate logic to the corresponding services, and return HTTP responses. JWT authentication (`[Authorize("APIAuth")]`) is applied in all controllers.

<div id="pt"></div>

---

# API Gateway de Pagamento \ ***6º Termo - LP1***

Este projeto implementa uma API Gateway de Pagamento fictícia, desenvolvida em ASP.NET Core 8.0, para gerenciar operações básicas de cartões e transações financeiras.

## Visão Geral

A API funciona como um ponto de entrada para operações de pagamento, oferecendo endpoints para validação de cartões, cálculo de parcelas, registro de transações, e consulta/modificação de seu status (confirmar ou cancelar).

## Funcionalidades Principais

*   **Validação de Cartão:** Verifica a validade do número do cartão em um banco de dados e identifica a bandeira (Visa, Mastercard, Elo) com base em um padrão simplificado.
*   **Cálculo de Parcelas:** Calcula o valor das parcelas de uma transação com base no valor total, taxa de juros e quantidade de parcelas.
*   **Processamento de Pagamento:**
    *   Registra uma nova transação no banco de dados com status "PENDENTE".
    *   Verifica a validade do cartão antes de registrar a transação.
*   **Consulta de Status da Transação:** Retorna o status atual de uma transação (PENDENTE, CONFIRMADO, CANCELADO).
*   **Confirmação de Pagamento:** Altera o status de uma transação de "PENDENTE" para "CONFIRMADO", com validações para evitar confirmações de transações já canceladas.
*   **Cancelamento de Pagamento:** Altera o status de uma transação de "PENDENTE" para "CANCELADO", com validações para evitar cancelamentos de transações já confirmadas.

## Tecnologias Utilizadas

*   **Banco de Dados:** MySQL (`MySql.Data` para conexão)
*   **Autenticação:** JWT Bearer (configurado via `Microsoft.AspNetCore.Authentication.JwtBearer`)
*   **Logging:** Serilog (`Serilog.AspNetCore`, `Serilog.Formatting.Compact`, `Serilog.Sinks.File`, `Serilog.Sinks.MySQL`, `Serilog.Sinks.MariaDB`) para registro de eventos e erros.
*   **Documentação da API:** Swagger/OpenAPI (`Swashbuckle.AspNetCore`)

## Modelos de Domínio

*   **`Cartao`**: Representa um cartão com `Numero` e `Validade`.
*   **`Pagamento`**: Representa uma transação de pagamento, incluindo `_ValorTotal`, `_TaxaDeJuros`, `_QtdeParcelas`, `_Cartao`, `_CVV` e `_Situacao` (um `enum`).
*   **`Situacao`**: Enumeração para os status da transação: `PENDENTE (1)`, `CONFIRMADO (2)`, `CANCELADO (3)`.
*   **`Parcela`**: Representa uma parcela de pagamento, com `_Parcela` (número da parcela) e `_Valor`.
  
### Serviços (`CartaoService.cs`, `PagamentoService.cs`)

*   **`CartaoService`**: Contém a lógica para `ValidarCartao`, que verifica a existência do número do cartão e sua validade na tabela `Cartao` do banco de dados.
*   **`PagamentoService`**: Contém a lógica de negócio para:
    *   `CalcularParcelas`: Realiza o cálculo de juros simples para as parcelas.
    *   `GravarPagamento`: Inserts a nova transação na tabela `Transacao`.
    *   `ConsultaSituacaoPagamento`: Consulta o status de uma transação pelo ID.
    *   `ConfirmarPagamento`: Atualiza o status da transação para `CONFIRMADO`, com validação de estado.
    *   `CancelarPagamento`: Atualiza o status da transação para `CANCELADO`, com validação de estado.

### Controllers (`CartoesController.cs`, `PagamentosController.cs`)

Estes controladores expõem os endpoints da API, recebem as requisições HTTP, delegam a lógica para os serviços correspondentes e retornam as respostas HTTP. A autenticação JWT (`[Authorize("APIAuth")]`) é aplicada em todos os controladores.
