# Javascript
> Boas práticas


## Sumário

- [Formatação](#formata%C3%A7%C3%A3o)
- [Escopo](#escopo)
- [Objeto literal](#objeto%20literal)
- [Referências](#refer%C3%AAncias)

[[⬆︎ Topo]](#sum%C3%A1rio)


## Formatação

- Use colchetes na mesma linha da sentença:

    Normalmente os programadores possuem divergências quanto à esse tema, em demais linguagens de programação, mas no javascript como o ponto e vírgula ao final é implícito, podemos ter alguns resultados inesperados, como no exemplo abaixo:

```
// bad - the function returns undefined and not an object with a name property
function func() 
{
    return
    {
        name: "Batman"
    }; 
}

// good
function func() { return {
    name: "Batman" };
}
```
[[⬆︎ Topo]](#formata%C3%A7%C3%A3o)

## Escopo
- Javascript não possui classes por padrão, ele não é orientado a objetos mas possui o conteito de herança baseada em protótipos.

```
// bad
var Bruno = new person()

// good - você pode invocar uma função através do método new
var bruno = new Person()
```

Com isso, use nomes de funções que definam *"classes"* começando com letra maiúscula (*upper camel case*) e funções dentro do escopo, que definem métodos, começando por minúscula com as junções de termo em maiúscula (*lower camel case*).

```
// bad 
function car( model, year, km ) {

  this.Model = model;
  this.Year = year;
  this.KM = km;
 
}
 
car.prototype.to_string = function () {
  return this.Model + " completou " + this.km + " km";
};

// good
function Car( model, year, km ) {

  this.model = model;
  this.year = year;
  this.km = km;
 
}
 
Car.prototype.toString = function () {
  return this.model + " completou " + this.km + " km";
};
 
// usage
var fiesta = new Car( "Ford Fiesta", 2009, 20000 );
var corsa = new Car( "Chevrolet Corsa", 2010, 5000 );
 
console.log( fiesta.toString() );
console.log( corsa.toString() );
```

Variáveis usadas para valores imutáveis dentro de todo o ciclo de vida do script, "constantes".
```
// bad
    var pi = 3.14,
        max_width = 800;

// good
    var PI = 3.14,
        MAX_WIDTH = 800;
```

[[⬆︎ Topo]](#escopo)

## Objeto literal
Usar objetos literais para definir uma estrutura de dados encapsulados ajuda bastante a estruturar bem o projeto e evitar conflitos entre nomes de funções, objetos e variáveis. É util também para modularizar seus scripts. Nome do objeto e dos métodos usando lower camel case.
```
// good
var dog = {
  name: "Benji",
  getName: function () {
    return this.name; 
  }
};
```

### Em casos específicos você pode precisar criar propriedades ou métodos em tempo durante a execução, veja os padrões recomendáveis:
Inicializando o objeto
```
var dog = {};
```
definindo propriedade
```
dog.name = "Tobi";
```
adicionando método
```
dog.getName = function () {
  return dog.name; 
};
```

No exemplo acima estou definindo os métodos após a criação do objeto, dentro do ciclo de vida do script. É possível também redefinir valores e comportamentos:
```
dog.getName = function () {
  return "Rex";
};
```

Podemos deletar métodos do objeto em memória
```
delete dog.name;
```
Adicionar mais propriedades ou métodos
```
dog.say = function () {
  return "Woof!"; 
};

dog.fleas = true;
```
[Objeto literal](#objeto%20literal)

[[⬅︎ Voltar para Boas Práticas]](https://github.com/mktvirtual/guides/tree/master/boas-praticas)


----------

## Referências

[Book: Build Better Apllications with Coding and Design Patterns | JavaScript Patterns Stoyan Stefanov, O'Reilly](http://shop.oreilly.com/product/9780596806767.do)

[Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)