# Anexo 1 - `let` vs. `var`

Em JavaScript, para se declarar uma variável, usa-se o `var`. Isto funciona bem, excepto em alguns casos interessantes:

## Exemplo 1 - Loops

Considera o código abaixo, que faz um ciclo `for` e imprime o valor de uma variável:

```javascript
function teste() {
  for (var i = 0; i < 5; i++) {
    var i = 10;
    console.log(i);
  }

  console.log(i);
}

teste();

// Imprime:
// - 10
// - 11
```

Se se tentar fazer código semelhante em Java ou c#, o código não é válido, porque se está a re-declarar uma variável. No entanto, em C, podemos escrever o seguinte código (válido):

```c
void main()
{
    for (int i = 0; i < 5; i++)
    {
        int i = 10;
        printf("%d", &i);
    }
}

// Imprime:
// - 10
// - 10
// - 10
// - 10
// - 10
```

Para compilar e testar, podem usar os seguintes comandos:

```bash
$ gcc teste.c -o teste
$ chmod +x teste
$ ./teste
```

Para alguns programadores, isto é confuso, e não é o esperado. O que se está a passar no exemplo de JavaScript, é que no segundo `var i = 10`, o `var` é ignorado.

Isto porque o `var` define uma variável em toda a função; é o conceito de _function scope_. C, c#, e Java, usam _block scope_, e no caso do c# e Java, não é permitido redeclarar uma variável, mesmo se seja dentro de um bloco de código (como um `if` ou um `for`).

## Exemplo 2 - Undefined? Erro?

Outra coisa mais "absurda" é que o seguinte código:

```javascript
function teste() {
  console.log(i);

  var i = 10;
}

teste();
```

Não dá erro, simplesmente imprime `undefined` na consola. Até no C isto dá erro por se estar a usar uma variável não definida.

No entanto, o JavaScript interpreta o código acima como:

```javascript
function teste() {
  var i = undefined;

  console.log(i);

  i = 10;
}

teste();
```

O mesmo acontece com funções, pode-se chamar funções definidas mais abaixo no código. Isto é conhecido como "hoisting", levar a _declaração_ de variáveis e funções para o topo do _scope_ onde estão presentes.

## Exemplo 3 - Loops, v2

Assumindo o seguinte:

```javascript
function teste() {
  for (var i = 0; i < 5; i++) {
    setTimeout(function() {
      console.log(i);
    }, 1000);
  }
}

teste();
```

Assumir-se ia que o resultado seria imprimir na consola os números inteiros de 0 a 4, depois de 1 segundo (1000 ms). No entanto, o resultado é sempre o número 5. Isto porque o valor da variável nunca é copiado para dentro da função do `setTimeout`, porque pertence à função `teste`. Logo, quando a função é executada, ela vai buscar o último valor que a variável `i` tinha, que é, `5`.

---

## Solução: `let`

Na versão "ES6", também conhecida como "ES2015", foi introduzida uma nova palavra-chave: `let`. O objetivo do `let` é também declarar variáveis, mas a principal diferença é que o _scope_ da variável é _apenas_ no bloco onde foi definido.

Isto significa que

- No primeiro exemplo, vai funcionar da mesma forma que no C, (mas depois dá erro porque o `i` já não está definido fora do ciclo `for`)
- No segundo exemplo, o código nem sequer executa (como esperado)
- No terceiro exemplo, aparecerão os números de 0 a 4, tal como esperado (mas isto é porque os valores da variável são capturados, ao contrário do `var`)

A única razão porque o `var` não foi alterado para funcionar desta forma, é porque isto iria deixar de ser compatível com código já existente. Uma das responsabilidades do TC39 (o comité que é responsável por definir as funcionalidades do JavaScript) é assegurar a retro-compatibilidade com sites anteriores.

## Conclusão

Deve-se usar (quase) sempre o `let`. São muito raros os casos em que o `var` é útil, e a compatibilidade com os browsers atuais (excepto Internet Explorer) é 100%.

## Aparte: `const`

Poderão ver código na internet com `const`. O `const` funciona da mesma forma que o `let` (sendo introduzido na mesma versão), mas ainda é mais restrito, porque não permite a modificação do valor da variável, depois desta ser declarada, ou seja:

```javascript
let a = 5;
a = 6; // OK
```

```javascript
const a = 5;
a = 6; // Erro - redefinição de 'a'.
```

Para não gerar confusões, sugiro ficarem-se pelo `let`. O `const` é usado por pessoas que já têm muita experiência, ou têm _use cases_ para esses problemas.
