---
title: Instalando a gem pg no Mac sem PostgreSQL local
description: Uma forma rápida de resolver graças ao Homebrew
date: 2020-08-10 07:00:52
category: dev
background: "#637a91"
---
Talvez você seja como eu e prefira suas aplicações rodando em containers [Docker](https://www.docker.com/) em vez de instalar ferramentas na máquina, deixando tudo organizado e atualizado com facilidade. 😉  Mas, se você desenvolve em [Ruby](https://www.ruby-lang.org/pt/), sabe que não dá pra escapar de instalar algumas gems no ambiente local, em especial a `gem pg` para utilizar o [PostgreSQL](https://www.postgresql.org/).

Particularmente, eu precisei trocar de máquina de desenvolvimento algumas vezes e me deparei com o mesmo problema na configuração da minha aplicação feita com Rails e PostgreSQL, apesar de ter um dockerfile simples e poucas dependências. O caso era problema de comunicação entre a **gem pg** e o diretório de instalação do PostgreSQL, que não podia ser encontrado.

Este erro ocorre pois algumas gems, como a citada, precisam de extensões compiladas na máquina. A forma mais transparente de resolver esse problema é a instalação completa do PostgreSQL server no ambiente local, que não me interessava, afinal de contas, eu só precisava apontar o client para meu container.

A seguir vai a forma como meu problema foi resolvido no Mac com o [Homebrew](https://brew.sh/index_pt-br).

## PostgreSQL e libpq

O pacote `libpq` é a API em C utilizada para o client do PostgreSQL, ou seja, é um conjunto de bibliotecas que permitem a comunicação de queries e resultados com o server PostgreSQL.

Para instalar isso no Mac, você precisa do Homebrew instalado e atualizado, e então:

```shell
$ brew install libpq
```

## Rubygems e bundler

Você deve então instalar a gem pg localmente com o parâmetro que aponta o diretório da libpq.

```shell
$ gem install pg -- --with-opt-dir="/usr/local/opt/libpq"
```

Se você também utiliza bundler para instalação de suas dependências, deve salvar a configuração de build apontando para libpq. Isto irá criar um diretório oculto na estrutura de projetos do seu bundler com um par de chave e valor, onde a chave é a opção de build e o valor é o diretório da sua instalação customizada.

```shell
$ bundle config --local build.pg --with-opt-dir="/usr/local/opt/libpq"
$ bundle install
```

Estes passos foram encontradas na seguinte [página](https://michaelrigart.be/install-pg-ruby-gem-without-postgresql/), e tomei a liberdade de compartilhar em português já que não tive muita sorte achando resultados na nossa língua.

Fonte: <https://michaelrigart.be/install-pg-ruby-gem-without-postgresql/> 👏  in case you read this, thanks a lot, this hopefully will help other developers reading in Portuguese [🇧🇷](https://emojipedia.org/flag-brazil/) [🇵🇹](https://emojipedia.org/flag-portugal/)