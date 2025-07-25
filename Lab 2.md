# LAB 2 — Consumindo uma API REST no ABAP com `cl_http_client`

## Objetivo do Lab

- Aprender a consumir uma API REST no ABAP.
- Entender o uso da classe `cl_http_client`.
- Trabalhar com requisições HTTP do tipo GET.
- Analisar respostas JSON.
- Mostrar os dados no `WRITE` ou `ALV` simples.
- Relacionar com o que foi aprendido no Postman.

---

## Parte 1 — Teoria

### O que é `cl_http_client`?

É uma classe padrão do SAP usada para fazer chamadas HTTP. Com ela, é possível:

- Enviar requisições para APIs externas.
- Receber respostas (geralmente em JSON).
- Ler status codes, cabeçalhos e corpo da resposta.
- Integrar o SAP com serviços REST (OData, APIs públicas, etc.).

---

## Etapas de uso no ABAP

1. Criar a instância HTTP com a URL da API.
2. Definir o método (GET, POST, etc.).
3. Enviar a requisição.
4. Verificar o status da resposta.
5. Obter o corpo (body) da resposta.
6. Tratar o JSON se necessário.

---

## Parte 2 — Exemplo Prático

Vamos consumir uma API pública de teste:

**URL:**  

``` https://jsonplaceholder.typicode.com/posts/1 ```

Essa API retorna um objeto JSON com título, corpo e ID do post.

---

## Código ABAP — GET com `cl_http_client`

### Programa no SE38 / SE80

```abap
REPORT zlab2_api_get.

DATA: lo_http_client TYPE REF TO if_http_client,
      lv_url         TYPE string,
      lv_response    TYPE string,
      lv_status      TYPE i.

lv_url = 'https://jsonplaceholder.typicode.com/posts/1'.

* 1. Criar o cliente HTTP
CALL METHOD cl_http_client=>create_by_url
  EXPORTING
    url                = lv_url
  IMPORTING
    client             = lo_http_client
  EXCEPTIONS
    argument_not_found = 1
    plugin_not_active  = 2
    internal_error     = 3
    OTHERS             = 4.

IF sy-subrc <> 0.
  WRITE: / 'Erro ao criar cliente HTTP'.
  RETURN.
ENDIF.

* 2. Configurar método como GET
lo_http_client->request->set_header_field( name = '~request_method' value = 'GET' ).

* 3. Enviar requisição
CALL METHOD lo_http_client->send
  EXCEPTIONS
    http_communication_failure = 1
    http_invalid_state         = 2
    http_processing_failed     = 3
    OTHERS                     = 4.

IF sy-subrc <> 0.
  WRITE: / 'Erro ao enviar requisição'.
  RETURN.
ENDIF.

* 4. Receber resposta
CALL METHOD lo_http_client->receive
  EXCEPTIONS
    http_communication_failure = 1
    http_invalid_state         = 2
    http_processing_failed     = 3
    OTHERS                     = 4.

IF sy-subrc <> 0.
  WRITE: / 'Erro ao receber resposta'.
  RETURN.
ENDIF.

* 5. Obter status e corpo
lo_http_client->response->get_status( IMPORTING code = lv_status ).
lo_http_client->response->get_cdata( RECEIVING data = lv_response ).

WRITE: / 'Status HTTP:', lv_status.
WRITE: / 'Conteúdo da resposta:', lv_response.
```

---

## Saída esperada

Quando executado, o programa deve exibir algo como:

```
Status HTTP: 200
Conteúdo da resposta: {"userId":1,"id":1,"title":"...","body":"..."}
```

---

## Explicações

- `create_by_url`: cria um objeto de conexão HTTP.
- `set_header_field`: define o método HTTP (GET).
- `send`: envia a requisição.
- `receive`: recebe a resposta.
- `get_status`: retorna o código HTTP (200, 404 etc.).
- `get_cdata`: retorna o corpo da resposta (normalmente JSON).

---

## Desafio Extra (Opcional)

- Tente trocar o ID do post na URL (`posts/1` → `posts/2`, etc.).
- Copie a saída JSON e valide no [https://jsonformatter.org/](https://jsonformatter.org/).
- Em labs futuros vamos aprender a **parsear o JSON** no ABAP com `cl_trex_json_deserializer`.

---

## Conclusão

Com este lab, você realizou o primeiro consumo real de uma API REST usando ABAP. Agora o SAP deixou de ser uma "caixa fechada" e começou a interagir com o mundo externo.

Próximo passo sugerido: aprender Open SQL básico para consultar dados internos do SAP.
