---
title: Instalando gem PG no Mac sem PostgreSQL local
description: change
date: 2020-08-10 07:00:52
category: dev
background: "#637a91"
---
Eu prefiro minhas aplicações rodando em containers Docker em vez de instalar ferramentas na minha máquina. No entanto, ao rodar minha aplicação Rails com PostgreSQL, mais de uma vez tive que lidar com o problema de comunicação entre a gem pg e o diretório de instalação do PostgreSQL, que não podia ser encontrado.

Para solucionar isso no Mac


```shell
$ brew install libpq
```

