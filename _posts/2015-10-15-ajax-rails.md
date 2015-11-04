---
layout: post
title:  "Rails, Ajax e formulários remotos."
date:   2015-10-15 01:23:36
categories: rails
author: "Anchieta Jr."
---

Olá, seja bem vindo a mais um post.

Hoje iremos abordar além de Ruby on Rails, um pouco de Ajax e Formulários remotos, peças fundamentais na construção de aplicações web interativas.

**Problema:**

Estou cadastrando um produto e ainda não existe categoria para este produto. Como cadastrar uma dependência (Categoria) do meu model (Produto), sem a necessidade de acessar a view de cadastro da minha categoria e perder os dados que já havia digitado no formulário de produto?

**Solução utilizada nesse post:**

Efetuar o cadastro da minha dependência (Categoria) via ajax através de um formulário remoto.

**Ajax ?**

"AJAX é acrônimo em língua inglesa de "Asynchronous Javascript and XML", que em português significa "Javascript e XML Assíncronos". Designa um conjunto de técnicas para programação e desenvolvimento web que utiliza tecnologias como Javascript e XML para carregar informações de forma assíncrona."

**[Leia mais...](http://www.significados.com.br/ajax/)**

Resumindo ao básico do básico no nosso caso, Ajax é uma técnica para efetuar requests a um servidor e receber a resposta desse request sem a necessidade de efetuar um **Reload** completo na página.

**Formulário Remoto ?**

Uma chamada a um formulário que não faz parte originalmente do contexto daquela view, pra que não seja necessário criar outro formulário com a mesma funcionalidade na aplicação, "chamamos" o nosso formulário remoto.

**Iniciando a aplicação**

Versão Ruby: **2.2.0**

Versão Rails: **4.2.2**

Criando a nossa aplicação e configurando, vamos pro terminal:

`rails new ajaxrails`

`cd ajaxrails`

`bundle install`

`rake db:create`

Em seguida, vamos gerar dois scaffolds, um pra categoria e outro pra produto.

`rails generate scaffold Category name:string`

`rails generate scaffold Product name:string color:string category:references`

`rake db:migrate`

Adicionar nosso relacionamento aos models: 

`app/models/product.rb`...

{% highlight ruby %}
class Product < ActiveRecord::Base
  belongs_to :category
end
{% endhighlight %}

... e `app/models/category.rb`.

{% highlight ruby %}
class Category < ActiveRecord::Base
  has_many :products
end
{% endhighlight %}

Adicionei isso ao `app/views/layouts/application.html.erb` só pra facilitar a navegação entre Produtos e Categorias.

{% highlight html %}
<style type="text/css" media="screen">
#menu{
	margin-bottom: 40px;
}
</style>
<div id="menu">
	<%= link_to "Produtos", products_path %>
	<%= link_to "Categorias", categories_path %>
</div>
{% endhighlight %}

Também adicionei isso ao `config/routes.rb` pra manter nossa view index de produtos como nossa página inicial.

{% highlight ruby %}
root 'products#index'
{% endhighlight %}









