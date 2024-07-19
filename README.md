# Tech Challenge 5 - Criação de um Ecommerce

Este repositório engloba os serviços referente ao sistema de ecommerce, sendo estes:

1. [Autenticação](https://github.com/AydanAmorim/ecommerce-user/tree/master)
2. [Carrinho](https://github.com/DFaccio/cart-service/tree/main) de compras
3. [Pagamento](https://github.com/erickmatheusribeiro/Payment-Microservice/tree/main)
4. [Produtos](https://github.com/fysabelah/spring-batch-products/tree/main): estoque e reserva

Também há o serviço de [descoberta de serviços](https://github.com/DFaccio/ecommerce-registry/tree/main) e
o [gateway](https://github.com/DFaccio/ecommerce-gateway/tree/main).

## Tecnologias

* Spring Boot para a estrutura do serviço
* Spring Data para manipulação de dados
* Spring Cloud para comunicação entre os serviços
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

1. Autenticação: http://localhost:7071/ecommerce/authentication-api
2. Carrinho: http://localhost:7071/ecommerce/shopping-cart
3. Pagamento: http://localhost:7071/ecommerce/payment
4. Produto: http://localhost:7071/ecommerce/inventory

Após os endereços mencionados basta adicionar o path desejado.

## Documentação

Considerando que a aplicação está sendo executada via container, as documentação podem ser acessadas conforme abaixo.

1. [Autenticação](http://localhost:7071/ecommerce/authentication-api/doc/user.html)
2. Carrinho
3. Pagamento
4. [Produto](http://localhost:7071/ecommerce/inventory/doc/products.html)

Caso a aplicação esteja rodando local, o swagger está disponível em http://localhost:
${server.port}/doc/${valor_customizado_serviço}, para este caso, sugiro verificar no repositório do serviço que deseja.

O docker também gerará a interface web para Postgres e Mongo, visto que as aplicações de pagamento e autenticação, e,
carrinho e produto, os utilizam respectivamente.

Caso deseja acessa as interfaces, basta clicar nos links abaixo.

* [Mongo](http://localhost:6061/)
    * Usuário e senha definido nas chaves ME_CONFIG_BASICAUTH_USERNAME e ME_CONFIG_BASICAUTH_PASSWORD do arquivo .env
      respectivamente.
* [Postegres](http://localhost:6063/login)
    * Usuário e senha definido nas chaves PGADMIN_DEFAULT_EMAIL e PGADMIN_DEFAULT_PASSWORD do arquivo .env
      respectivamente.

## Como executar

Crie um arquivo .env no diretório principal conforme abaixo.

```
# MongoDB
MONGO_INITDB_ROOT_USERNAME=ecommerce_admin
MONGO_INITDB_ROOT_PASSWORD=adm!n

# MongoDB Express
ME_CONFIG_BASICAUTH_USERNAME=admin
ME_CONFIG_BASICAUTH_PASSWORD=!nterf@ce#

# PostgreSQL
POSTGRES_DATABASE_USERNAME=postgres
POSTGRES_DATABASE_PASSWORD=root
POSTGRES_DATABASE_HOST=postgres

#PgAdmin
PGADMIN_DEFAULT_EMAIL=ecommerce@gmail.com
PGADMIN_DEFAULT_PASSWORD=serv1ce

# Gateway
# Quando container http://server-discovery:7070/eureka
# Quando localhost http://localhost:7070/eureka
EUREKA_SERVER=http://server-discovery:7070/eureka

PRODUCT_ADDRESS=lb://inventory
PAYMENT_ADDRESS=lb://payment
CART_ADDRESS=lb://cart-service
USER_ADDRESS=lb://ecommerce-user

# Caso o serviço de autenticação esteja rodando local, o valor é localhost:7073. Quando docker, user-service.
# gateway docker e ele local: host.docker.internal:7073
JWT_SERVER=user-service

# API
PROFILE=prod
```

Após criação do arquivo, execute o comando abaixo:

    docker compose up

### Dúvidas sobre os campos

* EUREKA_SERVER: quando o gateway estiver em container usar http://server-discovery:7070/eureka, quando localhost
  http://localhost:7070/eureka
* As chaves PRODUCT_ADDRESS, PAYMENT_ADDRESS e CART_ADDRESS referem-se ao serviços conectados no gateway. Caso não irá
  desenvolver, não precisa alterar nada. Caso tenha algum serviço rodando local, basta usar
  http://host.docker.internal:${PORTA_APLICAÇÃO_LOCAL}.
    * Exemplo: PRODUCT_ADDRESS=http://host.docker.internal:7072, significa que todos estão executando em container e o
      serviço de produtos está rodando localhost.

Obs.: Os serviços não ficam expostos quando executado via container, ou seja, todas as operações são obrigatoriamente
executadas via gateway.

* JWT_SERVER: devido o sistema de autenticação e autorização, é necessário informar o endereço que está a chave pública
  que gerou o token JWT. Neste caso as possibilidades são conforme mecionadas no arquivo:
    * ambos local: localhost:7073
    * gateway container e serviço local: host.docker.internal:7073
    * ambos container: user-service

Os demais campos referem-se a configuração de banco. Para mais informações sobre as chaves utilizadas no Gateway, favor
visitar o [README.md do projeto](https://github.com/DFaccio/ecommerce-gateway).

### Desenvolvimento

Os projetos possuem informações de como configurar quando rodando sozinhos ou parcialmente em container. Este também
contém para algumas variáveis.

O ponto de atenção principal seria se deseja rodar tudo local ou parcialmente em docker, o que faria mudar algumas
variáveis para o Gateway.

Nesta situação, basta alterar o endereço do serviço desejado conforme abaixo.

    http://host.docker.internal:${server.port}

Exemplo: considerando que irá desenvolver no projeto de produtos, então ficaria:

    http://host.docker.internal:7072

Obs.: _server.port_ é uma chave que pode ser encontrada no application.properties de cada serviço. 