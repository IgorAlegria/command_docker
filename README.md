# :whale2: Projeto Docker To do List :whale2:

O objetivo do projeto foi dockerizar a aplicação Todo App que já estava pronta. Para isso desenvolvemos arquivos com os comandos Docker CLI, arquivos Dockerfile e um arquivo docker-compose.

### Trybe Todo-App 🖥️ 📝

Olá! Esse é o aplicativo de tarefas **Trybe Todo-App**!

Com ele, você pode se organizar de maneira simples, adicionando, marcando e/ou removendo suas tarefas.

Uma verdadeira *mão-na-roda* para acompanhar seu progresso!

![intro](https://user-images.githubusercontent.com/106452876/213876138-134df37f-18ad-439d-8c90-d433cdd5db54.gif)

## Tecnologias usadas
Back-end:
> Desenvolvido usando: Docker, Dockerfile, docker-compose

## Instalando Dependências
> Frontend
```bash
cd ./todo-app/front-end
npm install
``` 
> Backend
```bash
cd ./todo-app/back-end
npm install
``` 
> Testes

:warning: Essa aplicação só funciona se associada a uma rede Docker;

## Executando aplicação
* Para rodar o front-end:

  ```
    cd ./todo-app/front-end && npm start
  ```
* Para rodar o back-end:

  ```
    cd ./todo-app/back-end && npm start
  ```
  
## Executando Testes

* Para rodar todos os testes:

  ```
    npm test
  ```
## Dockerfile
Para dockerizar a aplicação, foram criados três Dockerfile. Um para back-end, outro para front-end e outro para testes.
* Back-end:

  ```js
    FROM node:14

    EXPOSE 3001

    WORKDIR /back-end

    ADD node_modules.tar.gz ./

    COPY ./ ./

    ENTRYPOINT ["npm", "start"]
  ```
* Front-end:

  ```js
    FROM node:14

    EXPOSE 3000

    WORKDIR /front-end

    ADD node_modules.tar.gz ./

    COPY ./ ./

    ENTRYPOINT ["npm", "start"]
  ```
* Testes:

  ```js
    FROM mjgargani/puppeteer:trybe1.0

    WORKDIR /tests

    ADD node_modules.tar.gz ./

    COPY ./ ./

    ENTRYPOINT ["npm", "test"]
  ```
  :warning: Foi utilizado o ADD pois era preciso descompactar o arquivo ```node_modules.tar.gz``` e adicioná-lo em seguida no WORKDIR através do COPY.
  O ENTRYPOINT foi usado para garantir a execução do código.
  
  ## docker-compose
  ```js
    version: '3'
services:
  todotests:
    build: ./todo-app/tests
    depends_on:
     - todofront
     - todoback
    environment:
      - FRONT_HOST=todofront
    networks:
      - trybe-network-docker

  todofront:
    build: ./todo-app/front-end
    ports:
      - 3000:3000
    depends_on:
      - todoback
    environment:
      - REACT_APP_API_HOST=todoback
    networks:
      - trybe-network-docker

  todoback: 
    build: ./todo-app/back-end
    ports:
      - 3001:3001
    networks:
      - trybe-network-docker

networks:
  trybe-network-docker:
  ```
  > O build utiliza as imagens criadas pelo Dockerfile, o service todotest depende do todoback, todofront e o sevice todofront depende do todoback para executar e estão conectados por um network trybe-network-docker.
  
