---
title: Instalando gem PG no Mac sem PostgreSQL local
description: change
date: 2020-08-10 07:00:52
category: dev
background: "#637a91"
---
Talvez você seja como eu e prefere suas aplicações rodando em containers Docker em vez de instalar ferramentas na máquina, deixando tudo organizado e atualizado com facilidade. 😉  Mas, se você desenvolve em ruby, sabe que não dá pra escapar de instalar algumas gems no ambiente local, especialmente `gem pg` para utilizar o PostgreSQL.

Particularmente, eu precisei trocar de máquina de desenvolvimento algumas vezes e me deparei com o mesmo problema na configuração da minha aplicação feita com Rails e PostgreSQL, apesar de ter um dockerfile simples e poucas dependências. O caso era problema de comunicação entre a **gem pg** e o diretório de instalação do PostgreSQL, que não podia ser encontrado.

Este erro ocorre pois algumas gems, como a citada, precisam de extensões compiladas na máquina. A forma mais transparente de resolver esse problema é a instalação completa do PostgreSQL server no ambiente local, que não me interessava, afinal de contas, eu só precisava apontar o client para meu container.



## PostgreSQL e libpq

O pacote `libpq` é a API em C utilizada para o client do PostgreSQL, ou seja, é um conjunto de bibliotecas que permitem a comunicação de queries e resultados com o server PostgreSQL.

Para instalar isso no Mac, você precisa do Homebrew instalado e atualizado, e então:

```shell
$ brew install libpq
```



```shell
$ gem install pg -- --with-opt-dir="/usr/local/opt/libpq"
```