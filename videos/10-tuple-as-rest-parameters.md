### Tuple as rest parameters

Sabemos que usando JavaScript podemos chamar uma função utilizando a sintaxe de `spread`.

```tsx
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
```

Sabemos também que uma função pode ser declarada de forma a receber inúmeros parâmetros, utilizando uma sintaxe chamada `rest parameters`

```tsx
function sum(...args) {
  let sum = 0;
  for (let arg of args) sum += arg;
  return sum;
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4)); // 10
```

Até aqui falamos de duas funcionalidades que podem usadas no JavaScript, então com essa informação em mente, vamos ver se conseguimos fazer também a nível de tipagem.

**TypeScript**

No exemplo de código abaixo podemos ver que a tipagem de ambas funções é a mesma.

```tsx
type User = { 
  email: string;
  phone: string; 
}
const User = { email: 'john@email.com', phone: '12345678' }

function sendEmail(user: User, title: string, content: string) {}
function sendSMS(user: User, title: string, content: string) {}

sendEmail(john, 'hello', 'world');
sendSMS(john, 'hello', 'world');
```

Imagine que tenhamos uma nova função sendo criada, recebendo os mesmos parâmetros.

```tsx
type User = {
  email: string;
  phone: string;
}

function sendEmail(user: User, title: string, content: string) {}
function sendSMS(user: User, title: string, content: string) {}
function sendNotification(user: User, title: string, content: string) {}

sendEmail(john, 'hello', 'world');
sendSMS(john, 'hello', 'world');
sendNotification(user, 'hello', 'world');
```

Para reutilizar um pouco de código a nível de tipagem, o que pode ser feito é a criação de uma tupla.

```tsx
type User = { 
  email: string;
  phone: string; 
}
const john = { email: 'john@email.com', phone: '12345678' }

type SendParams = [User, string, string];

function sendEmail(args: SendParams) {}
function sendSMS(args: SendParams) {}
function sendNotification(args: SendParams) {}

// Complains that an array need to be passed. 👇
sendEmail(john, 'hello', 'world'); 
sendSMS(john, 'hello', 'world');
sendNotification(john, 'hello', 'world');
```

Se utilizarmos da maneira acima, teríamos de adaptar todas as chamadas pra utilizar um array, o que não é muito legal, não é o intuito, já que queremos simplesmente reutilizar as tipagens.

```tsx
function sendEmail(...args: SendParams) {
	const [user, title, content] = args;
}
function sendSMS(...args: SendParams) {}
function sendNotification(...args: SendParams) {}
```

Porem tivemos de adicionar uma linha na função para recuperar os valores de dentro de `args`.
Portanto, podemos utilizar aqui um `array destructuring`

```tsx
type User = { 
  email: string;
  phone: string; 
}
const john = { email: 'john@email.com', phone: '12345678' }

type SendParams = [User, string, string];

function sendEmail(...[user, title, content]: SendParams) {}
function sendSMS(...[user, title, content]: SendParams) {}
function sendNotification(...[user, title, content]: SendParams) {}

sendEmail(john, 'hello', 'world'); 
sendSMS(john, 'hello', 'world');
sendNotification(john, 'hello', 'world');
```

No entanto, ainda existe algo que pode ser melhorado, repare que quando olhamos pra nossa tipagem `SendParams`, não sabemos mais o significado de cada parâmetro, perdemos esta informação.

```tsx
type SendParams = [User, string, string];
```

Podemos recuperá-la adicionando o que chamamos de `alias` para cada elemento da tupla.

```tsx
type SendParams = [user: User, title: string, content: string];
```

Agora, olhando para nossa tipagem, já temos um entendimento melhor do que representa cada valor da nossa tupla.

Bom, e é assim que você pode utilizar das `tuplas` junto com `spread syntax` e `rest params` e `array destructuring` pra poder reutilizar um pouco mais de código. 👍