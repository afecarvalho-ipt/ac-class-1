# Argumentos variáveis (varargs)

Varargs, ou argumentos variáveis, é a capacidade de uma função de receber um número variável de argumentos, tal como o `React.createElement`. Esta funcionalidade foi introduzida no JavaScript na versão ES6, ou ES2015. Esta funcionalidade também tem o nome de "spread", ou "splat".

A sintaxe é `...nomeDoArgumento`, e dentro da função, o valor será **sempre** um array. Mesmo quando não se passam argumentos (em que aí, o array está vazio).

Podemos usar isto nas nossas funções, da seguinte forma:

```javascript
function soma(...numeros) {
  let total = 0;

  for (let n = 0; n < numeros.length; n++) {
    total += numeros[n];
  }

  return total;
}

soma(); // 0
soma(1); // 1
soma(2); // 2
soma(1, 2, 3); // 6
```

Também podemos passar arrays para estas funções, usando mais uma vez a sintaxe dos `...`:

```javascript
// Assumindo a função "soma" definida acima...

let nums = [1, 2, 3, 4, 5];

soma(...nums); // 15

nums.push(10);

soma(...nums); // 25

// Atenção: Isto é permitido, mas dá asneira!
soma(nums); // '01,2,3,4,5,10'
```

# Atenção!

Um argumento com número variável de valores tem que ser sempre o **último** da função. Não é permitido ter outros argumentos após este.

Ou seja:

```javascript
function soma(x, ...nums) {} // OK, precisa de pelo menos 1 argumento
function soma(x, y, ...nums) {} // OK, precisa de pelo menos 2 argumentos
function soma(...nums) {} // OK, zero ou mais

function soma(...nums, x) {} // ERRO
function soma(x, ...nums, y) {} // ERRO
```
