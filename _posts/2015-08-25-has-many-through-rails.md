---
layout: post
title:  "Associações Many-to-Many utilizando has_many e through."
date:   2015-08-25 20:40:36
categories: rails
---

Utilizar uma associação **many-to-many** em uma aplicação Rails com a ideia de ter uma lista de checkboxes gerada através de um dos Models, dando a opção de selecionar os itens desejados é o objetivo desse Post.

Existem algumas maneiras de implementar uma associação desse tipo no **Rails**, mas neste post será utilizado o `has_many` e `:through`, a forma mais recomendada nos posts do Stackoverflow que pesquisei, então vamos adiante.

Versão Ruby: **2.2.2**
Versão Rails: **4.2.0**

Criando a nossa aplicação e configurando, abra o terminal:

`rails new manytomany`

`cd manytomany`

`bundle install`

`rake db:create`

Precisaremos de 3 Models pra implementar nosso relacionamento has_many, vou utilizar o **scaffold** para os dois principais e gerar apenas o Model pra o nosso relacionamento, nesse exemplo trabalharei com os objetos Product, Category (Produto e Categoria), onde um produto pode ter várias categorias e uma categoria contém vários produtos:

`rails generate scaffold Product name:string`

`rails generate scaffold Category name:string`

`rake db:migrate`

`rails server`

Verifique se está tudo correto em : 

`localhost:3000/products` 

e ...

`localhost:3000/categories`

Agora, vamos criar nosso Model responsável pelo relacionamento entre nossos dois anteriores:

`rails g model Categorization product_id:integer category_id:integer`

Estou chamando o Model de **Categorization** pra ficar mais expressivo em relação aos outros dois Models e a relação que queremos criar.

Lembrando que é possível criar outros campos nesse Model caso necessário, se quiséssemos.

`rake db:migrate`

Agora vamos ao código, vamos configurar nossos Models e "ensina-los" a se relacionar:

`app/models/product.rb`

{% highlight ruby %}
class Product < ActiveRecord::Base
	has_many :categorizations
	has_many :categories, :through => :categorizations
end
{% endhighlight %}

`app/models/category.rb`

{% highlight ruby %}
class Category < ActiveRecord::Base
	has_many :categorizations
end
{% endhighlight %}

E nosso Model de relacionamento:

{% highlight ruby %}
class Categorization < ActiveRecord::Base
	belongs_to :producto
	belongs_to :category
end
{% endhighlight %}

Testar se está tudo funcionando via Console é uma boa, depois veremos nas Views:

`rails console`

Vamos cadastrar algumas categorias e um produto:

`c1 = Category.new`

`c1.name = "Celular"`

`c1.save`

`c2 = Category.new`

`c2.name = "Smartphone"`

`c2.save`

`p = Product.new`

`p.name = "iPhone 6"`

A partir daqui entenderemos a 'mágica', vamos adicionar algumas categorias a esse produto:

`p.category_ids = [1,2]`

`=> [1,2]`

Então nossa associação está funcionando, vamos salvar esse produto e testar de outra forma:

`p.save`

Agora vejamos outra forma de trazer as categorias relacionadas a esse produto, sem apenas trazer os seus ids, mas sim todas as informações:

`p.categories`

Deve ser mostrado algo do tipo:

`=> #<ActiveRecord::Associations::CollectionProxy [#<Category id: 1, name: "Celular", #<Category id: 2, name: "Smartphone">]>`

Isso significa que está tudo ok, podemos trabalhar nossas views:

`app/views/products/_form.html.rb`


{% highlight ruby %}
<%= hidden_field_tag "product[category_ids][]", nil %>
<% Category.all.each do |category| %>
  <%= check_box_tag "product[category_ids][]", category.id, 
  @product.category_ids.include?(category.id), 
  id: dom_id(category) %>
  <%= label_tag dom_id(category), category.name %>
<% end %>
{% endhighlight %}

`app/views/products/show.html.rb`

{% highlight ruby %}
<% @product.categories.each do |category| %>
	<p><%= category.name %></p>		
<% end %>
{% endhighlight %}

E aqui vai o pulo do gato :), vamos adicionar o nosso array de ids relacionados as Categorias aos nossos atributos em:

`app/controllers/products_controller.rb`

{% highlight ruby %}
	params.require(:product).permit(:name, categories_ids: [])
{% endhighlight %}

**et voilà. :)**

**Pesquisa: [RailsCasts](http://railscasts.com/episodes/47-two-many-to-many)**

**[Fonts on Github](https://github.com/anchietajunior/MANYTOMANY)**









