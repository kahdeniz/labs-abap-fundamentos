# LAB 1 — Infraestrutura, HTTP, APIs e Postman

## Objetivo do Lab

- Entender o básico sobre como sistemas se comunicam (cliente x servidor).
- Aprender o que é HTTP e como funcionam as requisições GET, POST, etc.
- Conhecer o que é API e para que serve.
- Aprender o que é JSON.
- Entender o que é DNS, portas e status codes.
- Aprender a usar o Postman para testar APIs.
- Relacionar tudo isso ao dia a dia no SAP (principalmente integrações).

---

## 1. O que é Infraestrutura? O que é um Servidor?

### O que é um Servidor?

- É **um computador** (ou conjunto deles) que guarda dados e programas.
- Ele **“espera”** que outros dispositivos (clientes) peçam informações a ele.
- Exemplo: um servidor web guarda páginas de sites, APIs ou sistemas como o SAP.

**No SAP:**

- Quando você faz login no SAP GUI, está se conectando a um servidor SAP.
- Quando integra SAP com outra aplicação (via API), seu SAP atua como cliente ou servidor.

---

## 2. Cliente x Servidor

- **Cliente** → quem faz pedidos (seu navegador, seu SAP, Postman, etc.).
- **Servidor** → quem responde com os dados pedidos.

**Analogia Simples:**

> Você (cliente) vai ao restaurante e faz seu pedido → o garçom (servidor) traz a comida (dados).

---

## 3. O que é HTTP e HTTPS?

- **HTTP** → Hypertext Transfer Protocol
  - Protocolo usado para troca de dados na web.
  - É como uma “linguagem” padrão que cliente e servidor usam pra se entender.

- **HTTPS** → HTTP Secure
  - Igual ao HTTP, mas os dados vão **criptografados**, para segurança.

**No SAP:**

- Integrações via API quase sempre usam HTTP ou HTTPS.
- Quando SAP chama uma API REST, geralmente é via HTTPS por segurança.

---

## 4. Métodos HTTP (GET, POST, PUT, DELETE)

Quando cliente e servidor conversam via HTTP, existem **métodos**. Os principais são:

| Método   | O que faz?                | Exemplo                            |
|----------|---------------------------|------------------------------------|
| GET      | Busca dados               | Ex.: buscar pedidos de vendas      |
| POST     | Cria dados novos          | Ex.: criar um novo cliente         |
| PUT      | Atualiza dados existentes | Ex.: atualizar endereço do cliente |
| DELETE   | Apaga dados               | Ex.: deletar um pedido             |

**No SAP:**

- Quando você consome uma API para buscar dados → normalmente usa GET.
- Para gravar dados em outro sistema → POST ou PUT.

---

## 5. O que é JSON?

- Significa **JavaScript Object Notation**.
- É um formato leve para troca de dados entre sistemas.
- Funciona como um texto, mas estruturado em chave e valor.

### Exemplo de JSON:

```json
{
  "cliente": "Empresa X",
  "pedido": 12345,
  "itens": [
    {
      "material": "ABC123",
      "quantidade": 10
    },
    {
      "material": "XYZ456",
      "quantidade": 5
    }
  ]
}
```

**No SAP:**

- Quando SAP consome ou expõe APIs, quase sempre trabalha com JSON.
- Você precisa saber ler e gerar JSON no ABAP quando faz integrações.

---

## 6. O que é REST?

- **REST** = Representational State Transfer.
- É um estilo de arquitetura para criação de APIs.
- Baseia-se em:
  - URLs bem definidas → cada recurso tem um “endereço”.
  - Métodos HTTP → GET, POST, etc.
  - Dados geralmente em JSON.

**No SAP:**

- SAP OData é baseado em REST.
- Integrações modernas com SAP usam APIs RESTful → chamadas simples via URL.

---

## 7. O que são Portas?

- Cada tipo de serviço na internet “ouve” numa **porta** específica.

| Porta | Serviço |
|-------|---------|
| 80    | HTTP    |
| 443   | HTTPS   |

**No SAP:**

- Quando configura uma conexão HTTP, às vezes precisa informar a porta.

---

## 8. O que é DNS?

- **DNS** = Domain Name System.
- Transforma nomes em endereços IP.
- Ex.: transforma `api.exemplo.com` em `192.168.10.20`

**No SAP:**

- Quando configura RFCs ou destinos HTTP, muitas vezes coloca o nome do servidor (FQDN) em vez de IP.

---

## 9. O que é API?

- API = Application Programming Interface.
- É uma “porta de entrada” para acessar dados ou funcionalidades de outro sistema.
- Ex.: SAP chama uma API de outro sistema para buscar preços de frete.

**Por que usar API?**

> Para integrar sistemas diferentes **sem precisar saber o código-fonte** de cada um.

---

## 10. O que é Status Code?

Quando você faz uma requisição HTTP, o servidor responde com um **status code**. Ex.:

| Código | Significado              |
|--------|--------------------------|
| 200    | OK                       |
| 404    | Não encontrado           |
| 500    | Erro interno no servidor |

---

## 11. O que é o Postman?

- Postman é uma ferramenta para **testar APIs**.
- Você monta requisições e vê as respostas do servidor.
- Excelente para aprender:
  - Ver dados JSON
  - Entender status code
  - Testar endpoints antes de programar no SAP

**No SAP:**

- Antes de fazer código ABAP para consumir API, testa-se no Postman se a API funciona.
- Evita gastar tempo programando algo que não responde.

---

# Parte Prática — Exercícios Guiados

## EXERCÍCIO 1 — Instalar o Postman

Baixe e instale o Postman:

- [Download Postman](https://www.postman.com/downloads/)

---

## EXERCÍCIO 2 — Fazer uma requisição GET

### Objetivo:

- Entender como funciona um GET e ver a resposta JSON.

### Passos:

1. Abra o Postman.
2. No campo “Enter request URL”, coloque:

```
https://jsonplaceholder.typicode.com/posts/1
```

3. Deixe o método como **GET**.
4. Clique em **Send**.
5. Veja o resultado no painel abaixo.

**Explicação:**

- O Postman fez um GET para um servidor gratuito.
- O servidor respondeu com dados JSON.

**Saída esperada:**

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit suscipit recusandae consequuntur expedita et cum..."
}
```

---

## EXERCÍCIO 3 — Trocar GET para POST

### Objetivo:

- Entender diferença entre GET e POST.

### Passos:

1. No mesmo Postman, mude o método para **POST**.
2. No campo Body, selecione **raw** e escolha JSON.
3. Digite:

```json
{
  "title": "Teste",
  "body": "Conteúdo de teste",
  "userId": 1
}
```

4. Clique em **Send**.

**Explicação:**

- POST envia dados novos ao servidor.
- Você está criando um “post”.

**Saída esperada:**

```json
{
  "title": "Teste",
  "body": "Conteúdo de teste",
  "userId": 1,
  "id": 101
}
```

---

## EXERCÍCIO 4 — Verificar Status Code

- Depois de enviar qualquer requisição no Postman:
  - Olhe no canto superior direito da resposta → lá aparece:
    - Status (200, 201, 404 etc.)
    - Tempo da resposta
    - Tamanho dos dados

**Importante para saber se a API deu certo ou deu erro.**

---

## EXERCÍCIO 5 — Ver o Network no Navegador

### Objetivo:

- Visualizar requisições HTTP na prática.

### Passos:

1. Abra o Chrome ou Edge.
2. Vá em qualquer site.
3. Aperte **F12** → abra o Developer Tools.
4. Vá na aba **Network**.
5. Recarregue a página.
6. Veja várias requisições (método GET, URLs, status code).

**Explicação:**

- Tudo o que você vê na web vem via requisição HTTP.
- No SAP, isso acontece também, só que “por trás” dos bastidores.

---

# Resumo — Por que isso importa no SAP?

- Para **consumir APIs via ABAP**, você precisa entender HTTP, métodos, JSON.
- Para resolver erros em integrações, precisa saber olhar status code, dados retornados.
- Postman é a ferramenta ideal para testar antes de codificar.

---
