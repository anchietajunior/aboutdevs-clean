---
layout: post
title:  "Iniciando com testes utilizando Rspec no Rails"
date:   2015-11-04 00:25:36
categories: rails
author: "Anchieta Jr."
---

Dessa vez vou abordar um tema legal pra quem é iniciante no Rails ou em testes, TDD e RSpec.

As vezes é complicado entender pra que serve e o motivo de se manter uma rotina de testes em uma aplicação, principalmente quando você testa na tela e vê que tudo está funcionando como você espera.

Mas como o Professor **[Maurício Aniche](https://github.com/mauricioaniche){:target="_blank"}** justifica, automatizar os testes da sua aplicação reduz o custo da mesma, por se tratar de testes feitos pela máquina e não por você, sempre que quiser saber se alguma funcionalidade do seu sistema está funcionando da forma desejada.

**Mas o que é TDD ?**

Test-Driven Development (TDD) ou Desenvolvimento Orientado a Testes de forma resumida é uma metodologia que visa cobrir toda ou boa parte de um sistema por testes e garantir o funcionamento correto de cada escopo definido em cada teste, o que traz qualidade ao que foi "escrito" através da **Refatoração**, que é uma das fases especificadas para a prática do TDD.
E não menos importante é a documentação do código, já que faz parte da prática do TDD especificar cada teste e o valor esperado para cada um.

Isso é um resumo do resumo, se quiser entender melhor sobre TDD, segue o link do site do professor Maurício Aniche. **[TDD](http://www.aniche.com.br/tdd/){:target="_blank"}**

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

Vou utilizar um escopo parecido com o do livro da **[EveryDayRails](http://everydayrails.com/){:target="_blank"}** que particularmente considero uma ótima leitura inicial pra quem quer entrar no mundo de testes automatizados com RSpec no Rails.

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

Pra criar alguns testes simples de validação, vamos colocar algumas validações em nosso 
`user.rb` em `app/models/`.

{% highlight ruby %}
class User < ActiveRecord::Base
   validates :firstname, presence: true
   validates :lastname, presence: true
   validates :email, presence: true, uniqueness: true
end
{% endhighlight %}

Essas validações tornam obrigatórias as presenças de nossos 3 campos e a unicidade do campo e-mail.

Voltando ao nosso `user_spec.rb` podemos descrever os testes que queremos criar utilizando o bloco it, de início vou descrever 2 e também vou deixar em **PT-BR** a descrição dos testes pra ajudar no entendimento, mas deixem em inglês quando fizerem os seus, até pra praticar o "English".

{% highlight ruby %}
require 'rails_helper'

describe User do
  it "é válido quando contém o primeiro nome, último nome e email"
  it "não é válido quando o primeiro nome é nulo"
end
{% endhighlight %}

Se executarmos nosso teste através do comando:

`rspec spec/models/user_spec.rb`

<image src="{{ site.baseurl }}/img/post-testes/spec.png"/>

Esse é o resultado que esperamos, porque nada foi implementado ainda nos testes, então vamos deixar o primeiro teste da seguinte forma e executar novamente:

{% highlight ruby %}
describe User do
  it "é válido quando contém o primeiro nome, último nome e email" do
	user = User.new(
	 firstname: 'Bruce',
	 lastname: 'Dickinson',
	 email: 'bruce@ironmaiden.com'
	)
	 expect(user).to be_valid
   end
end
{% endhighlight %}

Estamos criando um novo objeto do tipo User e atribuindo os valores ao firstname, lastname e ao email. Um padrão aconselhado e muito utilizado para escrever testes é o Arrange Act Assert (AAA), traduzindo ao pé da letra seria "Organizar, Agir e Afirmar".

  - (1) Organize todas as condições e entradas.
  - (2) Aja no objeto ou método sob o teste.
  - (3) Afirme que o resultado esperado aconteceu.

Leia mais em -> **[Arrange Act Assert](http://c2.com/cgi/wiki?ArrangeActAssert){:target="_blank"}**

Execute novamente `rspec spec/models/user_spec.rb`.


<image src="{{ site.baseurl }}/img/post-testes/spec2.png"/>

Agora, o teste passa e a mensagem de retorno indica que apenas o segundo teste continua pendente, vamos implementa-lo também.

{% highlight ruby %}
describe User do
    it "é invalido sem o primeiro nome" do
  	user = User.new(firstname: nil)
  	user.valid?
  	expect(user.errors[:firstname]).to include("can't be blank")
  end
end
{% endhighlight %}

Diferente do primeiro teste, este segundo "afirma" uma advertência ao final, no método expect estamos indicado que o resultado caso o firstname seja nulo é o aviso de validação "can't be blank" ou "não pode ser nulo".

No primeiro teste esperávamos que tudo ocorresse bem quando o usuário digite todos os campos de forma correta, agora estamos tratando um caso em que isso não ocorra.

Após a criação do objeto, perguntamos ao Rails se o objeto é válido através do método `valid?` e criamos nossa `expect` trazendo o alerta `(user.errors)` no campo firstname e a mensagem que deve ser gerada caso ocorra.

Agora um último teste aproveitando que estamos validando o campo email e informando ao sistema que não podem existir dois usuários com a mesma conta de e-mail.

Da mesma forma que os outros, começamos pelo bloco it com o comentário sobre o código e seguindo nosso formato AAA.


{% highlight ruby %}
describe User do
    it "é inválido caso já exista um e-mail igual cadastrado" do 
    user = User.create(
      firstname: 'Steve', 
      lastname: 'Harris', 
      email: 'contato@ironmaiden.com'
    )
    
    user = User.new(
      firstname: 'Bruce', 
      lastname: 'Dickinson', 
      email: 'contato@ironmaiden.com'
    )
    user.valid?
    expect(user.errors[:email]).to include('has already been taken')
  end
end
{% endhighlight %}

E mais uma vez executando `rspec spec/models/user_spec.rb`.

<image src="{{ site.baseurl }}/img/post-testes/spec3.png"/>

Os 3 testes passam. Se alterarmos a expect `to` para `not_to`, que espera o inverso do outro resultado, podemos validar se o teste realmente está fazendo o que queremos.

{% highlight ruby %}
describe User do
    it "é inválido caso já exista um e-mail igual cadastrado" do 
    user = User.create(
      firstname: 'Steve', 
      lastname: 'Harris', 
      email: 'contato@ironmaiden.com'
    )
    
    user = User.new(
      firstname: 'Bruce', 
      lastname: 'Dickinson', 
      email: 'contato@ironmaiden.com'
    )
    user.valid?
    expect(user.errors[:email]).not_to include('has already been taken')
  end
end
{% endhighlight %}

<image src="{{ site.baseurl }}/img/post-testes/spec4.png"/>

Agora ele falha, o que nos garante que realmente está sendo validado aquilo que queremos e que nosso teste está nos auxiliando a criar uma situação real que pode acontecer no nosso sistema.

É interessante que seja estudado mais intensamente as expectations e os tipos que o RSpec oferece pra que a prática do TDD possa cobrir uma gama maior de código, aumentando a qualidade do código, garantindo que esteja funcional.

Pra começar no mundo dos testes, ta valendo !

É isso ai, até a próxima.

**Agradecimentos ao mestre @jacksonpires que me indicou o TDD para estudos.**



