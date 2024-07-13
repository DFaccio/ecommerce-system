# Tech Challenge 5 - Criação de um Ecommerce

Este repositório engloba os serviços referente ao sistema de ecommerce, possui 3 principais serviços:
1. [Produtos](https://github.com/fysabelah/spring-batch-products/tree/main): estoque e reserva
2. [Pagamento](https://github.com/erickmatheusribeiro/Payment-Microservice/tree/main),
3. [Carrinho](https://github.com/DFaccio/cart-service/tree/main) de compras

A autenticação está embutida no serviço de gateway.

## Tecnologias
* Spring Boot para a estrutura do serviço
* Spring Data para manipulação de dados dos pedidos
* Spring Cloud para comunicação baseada em eventos com outros microsserviços
* PostgreSQL e MongoDB para persistência
* Spring Security

## Desenvolvedores

- [Aydan Amorim](https://github.com/AydanAmorim)
- [Danilo Faccio](https://github.com/DFaccio)
- [Erick Ribeiro](https://github.com/erickmatheusribeiro)
- [Isabela França](https://github.com/fysabelah)

## Acesso aos serviços

O [Gateway](https://github.com/DFaccio/ecommerce-gateway/tree/main) possui uma rota base chamada _ecommerce_, portanto,
os serviços devem ser acessado através de:
1. Produto: http://localhost:7071/ecommerce/inventory
2. Pagamento: http://localhost:7071/ecommerce/payment
3. Carrinho: http://localhost:7071/ecommerce/shopping-cart

Após os endereços mencionados basta adicionar o path desejado.

### Desenvolvimento

O acesso entre serviços é feito via _Feign Client_, desta forma, seria adicionar o nome definido em `application.properties`
na chave `spring.application.name` na anotação _@FeignClient_.

## Acesso a documentação

Todos serviços possuem swagger, porém não foi possível liberar o swagger-ui. No entanto, basta acessar a URL base mencionado
em **Acesso aos serviços** e adicionar `/documentation` que um json será retornado.

## Como executar

Crie um arquivo .env no diretório principal conforme abaixo.

```
# MongoDB
MONGO_INITDB_ROOT_USERNAME=ecommerce_admin
MONGO_INITDB_ROOT_PASSWORD=senha_mongo

# MongoDB Express
ME_CONFIG_BASICAUTH_USERNAME=admin
ME_CONFIG_BASICAUTH_PASSWORD=senha_interface_mongo

# PostgreSQL
DATABASE_USERNAME=postgres
DATABASE_PASSWORD=senha_postgres
DATABASE_HOST=postgres

#PgAdmin
PGADMIN_DEFAULT_EMAIL=ecommerce@gmail.com
PGADMIN_DEFAULT_PASSWORD=senha_pg_admin

# Gateway
# Quando container http://server-discovery:7070/eureka
# Quando localhost http://localhost:7070/eureka
EUREKA_SERVER=http://server-discovery:7070/eureka

PRODUCT_ADDRESS=lb://spring-batch-products #http://host.docker.internal:7072
PAYMENT_ADDRESS=lb://payment
CART_ADDRESS=lb://cart-service

# API
PROFILE=prod
```

Após criação do arquivo, execute o comando abaixo:
    
    docker compose up

### Dúvidas sobre os campos
 * EUREKA_SERVER: quando o gateway estiver em container usar http://server-discovery:7070/eureka, quando localhost
   http://localhost:7070/eureka
 * As chaves PRODUCT_ADDRESS, PAYMENT_ADDRESS e CART_ADDRESS referem-se ao serviços conectados no gateway. Caso não irá
   desenvolver não precisa alterar nada. Caso tenha algum serviço rodando local, basta usar 
   http://host.docker.internal:${PORTA_APLICAÇÃO_LOCAL}.
   * Exemplo: PRODUCT_ADDRESS=http://host.docker.internal:7072, significa que todos estão executando em container e o 
   serviço de produtos está rodando localhost.


Obs.: Os serviços não ficam expostos quando executado via container, ou seja, todas as operações são obrigatoriamente 
executadas via gateway.