# Aula 1 - Relembrar o DOM, e introdução ao React

## Objetivos

- Relembrar as operações básicas do DOM
- Introduzir o React

## O DOM

O DOM, Document Object Model, é a API (Application Programming Interface) que permite aos programadores ler e manipular os conteúdos de uma página. O HTML (e XML) definem os seus conteúdos como árvores, existindo portanto uma relação pai-filho entre elementos.

### Leitura

Por "ler", assume-se obter um determinado elemento na página, através:

- Do `id` do elemento: `document.getElementById("idDoElemento")`, para obter **um** elemento com o ID pretendido.
- Do nome da `tag` (ex: `p`): `document.getElementsByTagName("p")`, para obter **todos** os elementos com a tag pretendida
- De uma ou mais `class`es: `document.getElementsByClassName("conteudo")`, para obter **todos** os elementos com a classe pretendida
- De um selector CSS (ex: `p#mensagem`):
  - `document.querySelector(".conteudo")`, caso se pretenda obter apenas um elemento
  - `document.querySelectorAll(".conteudo")`, caso se pretenda obter vários elementos

Ou ler os valores de atributos de um elemento, como:

- `let nome = txtNome.value`

(Explicarei o `let` vs `var` mais à frente)

### Manipulação

Por "manipular", assume-se definir atributos, como:

- `elemento.style.color = "blue"`
- `elemento.readOnly = true`
- `txtNome.setAttribute("type", "text")`

Ou manipular mesmo os conteúdos da página, como:

- Manipular o texto de um elemento: `elemento.textContent = "Mensagem"`
- Manipular o HTML de um elemento:
  - Definir: `elemento.innerHTML = "<p>Olá</p>"`
  - Concatenar: `elemento.insertAdjacentHTML("beforeend", "<p>Olá</p>")` (nota: não usar o `innerHTML +=` por questões de desempenho)
- Remover um elemento do DOM:
  - Remover-se a si próprio: `elemento.remove()`
  - Remover a partir do "pai": `pai.removeChild(elemento)`
- Adicionar um elemento a outro:
  - Via manipulação do HTML (ver acima)
  - Ou via `pai.appendChild(elemento)`

Ou ainda definir "event listeners", para se poder reagir a interações na página:

```html
<button id="teste">Teste</button>
```

```javascript
let btn = document.querySelector("#teste");

// Opção 1: Usar .onXYZ para definir o evento
btn.onClick = evt => {
  alert("click");
};

// Opção 2: Usar .addEventListener
btn.addEventListener("click", evt => {
  alert("click");
});
```

Atenção que as duas opções não são 100% equivalentes, apesar de aparentarem fazer o mesmo:

- `.onXYZ` regista a função para ser executada no evento. Se já existir algum "event listener" no elemento, este é substituído.
- `.addEventListener` adiciona um "event listener" no elemento. Todos os que estão lá, ficam lá. Significa que poderão cair no erro de ter a mesma função a ser executada múltiplas vezes.

```javascript
// As duas opções acima podem também ser escritas desta forma:
function onBtnClick(evt) {
  alert("click");
}

// Opção 1.1: Usar .onXYZ para definir o evento
btn.onClick = onBtnClick;

// Opção 2.1: Usar .addEventListener
btn.addEventListener("click", onBtnClick);
```

> Nota 1: Atenção à ausência dos parêntesis. Em JavaScript, funções são objetos, o que significa que podem ser guardadas em variáveis, e passadas como parâmetro para outras funções, tal como números, ou texto (strings). É, portanto, diferente fazer `btn.addEventListener('click', onBtnClick);` e `btn.addEventListener('click', onBtnClick());`. Geralmente, quer-se a primeira variante, e só em casos muito avançados é que se quer a segunda variante. A segunda variante, neste exemplo, iria imediatamente mostrar o `alert()`, e os cliques no botão não fariam nada.

> Nota 2: esqueçam usar `onclick="onBtnClick(this)"` diretamente no HTML. É simplesmente má ideia. Uma das razões é porque as funções têm que ser globais para poderem funcionar (e não se deve colocar código no objeto global).

> Nota 3: é possível remover eventos usando `elemento.removeEventListener('nomeDoEvento', funcaoRegistada)`.

```javascript
// Dada a função e botão definidos anteriormente...
btn.removeEventListener("click", onBtnClick);

// Mais info: https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener
```

## React

O React é uma biblioteca JavaScript com o objetivo de simplificar a construção de interfaces para o utilizador (user interfaces--UI).

Permite definir, de forma _declarativa_, o conteúdo e interações de uma parte ou da totalidade de uma interface gráfica.

> Nota: quando se diz que algo é _declarativo_, em programação, está-se a dizer que a forma de programar é mais semelhante ao que _quer_, enquanto que programação _imperativa_ é o "clássico": instruções para um computador, estamos a dizer _como é que se faz_.
> Associado ao modelo _imperativo_ vs. _declarativo_ também se ouve o termo _programação funcional_. Falar-se-á de alguns conceitos de programação funcional em aulas futuras.

### Objetivos do React

- Simplificar a criação de interfaces de utilizador com interações complexas e dinâmicas;
- Aplicar conceitos de Programação Orientada a Objetos (encapsulamento, herança) para definir "blocos" reutilizáveis (chamados de _componentes_) e organizar a estrutura da aplicação;
- Reduzir ou eliminar a necessidade do programador ter que manipular o conteúdo da página manualmente (usando, entre outras, as operações enumeradas acima);

### _Onde_ é que se pode aplicar o React

- Programação web (via `react-dom`)
- Programação para dispositivos móveis (via `react-native`)
- Programação para realidade virtual (via `react-vr`)

### Conceitos comuns

Existem alguns conceitos que serão usados durante as aulas:

#### Componentes

Um componente React é a definição de uma parte de uma interface de utilizador. Fazendo a analogia a P.O.O., um componente React é responsável por:

- Definir o conteúdo da interface gráfica;
- Reagir a eventos do utilizador, ou externos;
- Comunicar com outros elementos da UI.

Um componente pode ser definido com uma classe (`class`) ou uma função (`function`) JavaScript.

#### Elementos

Elementos são as instâncias dos componentes. A UI é composta por elementos, que são o resultado da instanciação e configuração dos componentes. Mais uma vez, fazendo a analogia a P.O.O., o elemento é o objeto.

#### Props (1)

Props (em português, propriedades), são os _argumentos_ que são "passados" para um componente React, de forma a criar elementos. No contexto da programação, `props` são os argumentos passados para uma função.

Sendo argumentos, não são editáveis (são, portanto, _imutáveis_).

> (1) Mais informações sobre _props_ serão dadas nas aulas seguintes.

#### State (e setState) (2)

Da mesma forma que uma classe permite definir atributos e métodos, um componente React também pode definir atributos (e métodos). Componentes React têm um atributo especial, chamado `state`. Alterações ao `state` são feitos através de uma função chamada `setState`.

> (2) Mais informações sobre _state_ serão dadas nas aulas seguintes.

### Funções importantes

React tem algumas funções importantes que servem como o "core" do seu funcionamento:

#### `React.createElement`

```typescript
class React {
  static createElement(
    component: string | Component,
    props: object | null,
    ...children: React.Node
  ): React.Element;
}
```

O que está escrito acima é a declaração da função `createElement`. Esta função tem um número variável de argumentos:

1. `component`: Representa o _nome_ do elemento (usado no HTML, ex: `"div"`), ou a `class`/`function` que representa o componente, para o React criar o elemento a partir dessa definição.
2. `props`: As `props` que serão passadas para o componente. Se não existem props a passar, deve-se usar `null`. Caso contrário, usa-se um objeto (ex: `{ type: "text" }`)
3. `...children`: A notação `...nomeDoArgumento`, em JavaScript, significa **zero ou mais valores**. Significa que este argumento aceita 0, 1, 2, ..., n valores, separados por vírgula. No contexto desta função, isto representa o _conteúdo_ do elemento. Caso não haja conteúdos, não se passa qualquer valor neste argumento.

Esta função devolve um elemento React.

##### Alguns exemplos

Ter:

```javascript
let exemplo = React.createElement("p", null, "Olá, React");
```

Resulta em:

```html
<p>Olá, React</p>
```

E seria o mesmo que:

```javascript
let exemplo = document.createElement("p");
exemplo.textContent = "Olá, React";
```

---

Ter:

```javascript
let exemplo = React.createElement("img", {
  src: "imagens/gato.jpg",
  alt: "Tareco"
});
```

Resulta em:

```html
<img src="imagens/gato.jpg" alt="Tareco" />
```

E seria o mesmo que:

```javascript
let exemplo = document.createElement("img");
exemplo.src = "imagens/gato.jpg";
exemplo.setAttribute("alt", "Tareco");
```

> Notar que aqui não se usou o argumento do `children`. Isto faz com que o `<img />` fique sem elementos-filho.

---

Ter:

```javascript
let exemplo = React.createElement(
  "p",
  null,
  "Olá, React",
  React.createElement("br", null),
  React.createElement("span", null, "Isto fica noutra linha")
);
```

Resulta em:

```html
<p>
  Olá, React
  <br />
  <span>Isto fica noutra linha</span>
</p>
```

E seria o mesmo que:

```javascript
// Criar o conteúdo inicial
let exemplo = document.createElement("p");
exemplo.textContent = "Olá, React";

// Adicionar a quebra de linha
let quebra = document.createElement("br");
exemplo.appendChild(quebra);

// Adicionar a outra mensagem depois da quebra de linha
let outraMsg = document.createElement("span");
outraMsg.textContent = "Isto fica noutra linha";
exemplo.appendChild(outraMsg);
```

> Notar aqui o facto de não só se ter usado múltiplos valores para o `children`, como o `children permite usar também outros elementos React, além de texto (strings) ou números. Isto permite construir hierarquias de elementos complexas.

#### `ReactDOM.render`

Esta função é específica ao `react-dom`. Permite colocar um _elemento_ React num determinado elemento HTML, **substituíndo o conteúdo anterior pelo conteúdo descrito pelo elemento**.

```typescript
class ReactDOM {
  static render(element: React.Element, node: DOM.Element): void;
}
```

É usada da seguinte forma:

```javascript
let exemplo = React.createElement("p", null, "Olá, React");

// Assumindo que existe um elemento com o ID "app" no HTML...
ReactDOM.render(exemplo, document.getElementById("app"));
```

### Criação e utilização de componentes

Um componente, na sua forma mais básica, é uma função JavaScript, que tem um único argumento, `props`, e devolve um elemento React:

```javascript
function OlaReact(props) {
  return React.createElement("p", null, "Olá, React");
}
```

E usa-se assim:

```javascript
let exemplo = React.createElement(OlaReact, null);
```

Usar, com o `ReactDOM.render`, é igual ao exemplo anterior.

---

Para um componente parametrizável, usamos o argumento `props` para ler e escrever valores:

```javascript
// Vamos assumir que "Ola" precisa de uma propriedade para o nome.
function Ola(props) {
  let resultado = React.createElement("p", null, "Olá, " + props.nome);

  return resultado;
}

// Para usar, usa-se da mesma forma que outro componente qualquer:
let olaReact = React.createElement(Ola, { nome: "React" });
let olaMundo = React.createElement(Ola, { nome: "Mundo" });

// Se quiser combinar os dois no mesmo ecrã, podemos juntar todos os exemplos anteriores, e ter o seguinte:

let exemplo = React.createElement("div", null, olaReact, olaMundo);

ReactDOM.render(exemplo, document.getElementById("app"));
```

O código acima resulta no seguinte HTML dentro do elemento com ID "app":

```html
<div>
  <p>Olá, React</p>
  <p>Olá, Mundo</p>
</div>
```

> "Pergunta para 1 milhão de €": O que acontece se passar `null` no argumento das `props` quando uso o `React.createElement`?
> Isto é, se fizer `React.createElement(Ola, null)`?
