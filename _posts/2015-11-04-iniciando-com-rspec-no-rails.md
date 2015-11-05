---
layout: post
title:  "Iniciando com testes utilizando Rspec no Rails"
date:   2015-11-04 00:25:36
categories: rails
author: "Anchieta Jr."
---

Dessa vez vou abordar um tema legal pra quem é iniciante no Rails ou em testes, TDD e RSpec.

As vezes é complicado entender pra que serve e o motivo de se manter uma rotina de testes em uma aplicação, principalmente quando você testa na tela e vê que tudo está funcionando como você espera.

Mas como o Professor **[Maurício Aniche](https://github.com/mauricioaniche)** justifica, automatizar os testes da sua aplicação reduz o custo da mesma, por se tratar de testes feitos pela máquina e não por você, sempre que quiser saber se alguma funcionalidade do seu sistema está funcionando da forma desejada.

**Mas o que é TDD ?**

Test-Driven Development (TDD) ou Desenvolvimento Orientado a Testes de forma resumida é uma metodologia que visa cobrir toda ou boa parte de um sistema por testes e garantir o funcionamento correto de cada escopo definido em cada teste, o que traz qualidade ao que foi "escrito" através da **Refatoração**, que é uma das fases especificadas para a prática do TDD.
E não menos importante é a documentação do código, já que faz parte da prática do TDD especificar cada teste e o valor esperado para cada um.

Isso é um resumo do resumo, se quiser entender melhor sobre TDD, segue o link do site do professor Maurício Aniche. **[TDD](http://www.aniche.com.br/tdd/)**

**O que é RSpec e pra que serve ?**

Também de forma bem resumida, RSpec é uma biblioteca (gem) que facilita a prática de testes em aplicações Ruby ou Rails, oferecendo uma sintaxe de fácil compreensão e uma gigante quantidade de opções pra escrever e executar testes em uma aplicação.

**Observação importante**

Praticamente todas as referências que li ou assisti falam da importância de se ter um bom conhecimento de programação e orientação a objetos pra escrever testes de forma coeza e que simplesmente auxilie a manter um código de boa qualidade, até porque se fosse pra atrapalhar não faria sentido estudar sobre isso. :D

Então, vou criar uma aplicação simples e também tentar escrever alguns testes simples pra começar, talvez futuramente volte a escrever mais sobre isso com testes mais complexos.

Go to the terminal.

`rails new testsapp`

`cd testapp`

Abra o arquivo Gemfile e adicione o rspec.

{% highlight ruby %}
gem "rspec-rails", "~> 3.1.0"
{% endhighlight %}

`bundle install`

Pra inicializar o diretório do rspec, execute o install.

`rails generate rspec:install`

Agora pode ser observado no projeto a pasta spec e sua estrutura, que ainda conta com os arquivos rails_helper.rb e spec_helper.rb.

Vou utilizar um escopo parecido com o do livro da **[EveryDayRails](http://everydayrails.com/)** que particularmente considero uma ótima leitura inicial pra quem quer entrar no mundo de testes automatizados com RSpec no Rails.

Vamos gerar um scaffold pra que seja nossa base pra criação dos testes, vou utilizar um model chamado User.

`rails generate scaffold User firstname lastname email`

`rake db:migrate`

Agora podemos começar a criar nossos testes.

**Obs: O método correto para prática do TDD é criar o teste antes de criar nosso model, rodar o teste e garantir que o mesmo está quebrado, essa é uma das especificações do TDD, no qual o teste tem de ser escrito antes do código, mas já que o RSpec gera nosso spec junto com o model, vamos fugir um pouco disso e focar nos testes, mas aconselho que sigam o fluxo correto.**

Dentro da pasta spec, existe uma outra pasta chamada models, dentro dela deverá existir um arquivo ruby chamado `user_spec.rb`, é nesse arquivo que escreveremos os testes para nosso model, testes de controller não serão abordados nesse texto, talvez futuramente. :)

Abra o arquivo, ele deverá estar da seguinte forma:

{% highlight ruby %}
require 'rails_helper'

RSpec.describe User, :type => :model do
  pending "add some examples to (or delete) #{__FILE__}"
end
{% endhighlight %}

Podemos deixar da seguinte forma, não irá alterar no resultado que esperamos:

{% highlight ruby %}
require 'rails_helper'

describe User do
  pending "add some examples to (or delete) #{__FILE__}"
end
{% endhighlight %}

Fica bem mais intuitivo e fácil de entender.







