# Funções são objetos

JavaScript é uma linguagem orientada a objetos, ainda mais que o Java. Isto porque, em JavaScript, funções **são objetos**.

Isto significa que:

## Posso guardar uma função numa variável:

Geralmente, para definir uma função, faço:

```javascript
function soma(n1, n2) {
  return n1 + n2;
}
```

No entanto, posso também fazer isto:

```javascript
let soma = function(n1, n2) {
  return n1 + n2;
};
```

Para o JavaScript, isto é quase equivalente, existindo apenas diferenças na forma como a variável é declarada (sugestão, não usem funções antes de serem declaradas).

## Posso passar uma função por parâmetro para outra função:

Quando se usam event listeners em JavaScript, uma das formas de registar o event listener (por exemplo, o código a ser executado no contexto de um `click`), estamos a passar funções por parâmetro:

```javascript
let btn = document.querySelector("button");

btn.addEventListener("click", function(evt) {
  alert("Click!");
});
```

Ao contrário do que pode parecer, **não estamos a invocar a função**. O que estamos a fazer é a dar a oportunidade que a função seja executada... eventualmente.

O mesmo se aplica quando se usam as funções como o `setTimeout`, `$.on`, `$.click` etc.

E o mesmo se aplica com componentes React:

```javascript
function Ola(props) {
  return React.createElement("p", null, "Olá");
}

ReactDOM.render(
  // Não invoquei a função Ola, só a passei por parâmetro,
  // como se fosse um número ou uma string.
  React.createElement(Ola, null),
  document.body
);
```
