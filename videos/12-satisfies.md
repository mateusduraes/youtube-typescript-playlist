Recentemente foi lançado o TypeScript 4.9, e uma nova feature que chamou muita atenção é o operador `satisfies`.

Esse operador vai permitir que você verifique que uma variável satisfaz a um tipo, sem que você altere qual tipo o TypeScript inferiu praquela variável.

### Exemplo código

Portanto, vamos observar o código abaixo

```tsx
function sendEmail(email: string) {}

type User = {
  name: string;
  email: string | null;
}

const user = {
  name: "john",
  email: 'john@dev.com'
}

```

Temos um objeto e uma tipagem, no entanto não estamos utilizando a tipagem, repare que, o `TypeScript` irá inferir o tipo da `const user`.

```tsx
const user: {
    name: string;
    email: string;
}
```

### Erros indesejados

Um possível problema para se atentar é que criar o objeto dessa forma tão livre, pode nos levar a cometer erros indesejados.

```tsx
const user: {
    name: string;
    mail: string;
}
```

Desconsiderando os possiveis problemas, nosso código funciona normalmente, podemos invocar a função `sendEmail` pois ela recebe uma string.

```tsx
function sendEmail(email: string) {}

type User = {
  name: string;
  email: string | null;
}

const user = {
  name: "john",
  email: 'john@dev.com'
}

sendEmail(user.email);
```

O ideal, já que estamos utilizando TypeScript, é verificar que uma variável tenha o tipo esperado.

```tsx
function sendEmail(email: string) {}

type User = {
  name: string;
  email: string | null;
}

const user: User = {
  name: "john",
  email: 'john@dev.com'
}

sendEmail(user.email);
```

Agora temos a certeza de que o a `const user` irá atender as especificações do `type User`.
No entanto, dentro do `type User` , o email pode ser null, então passamos a ter um problema na hora de chamar a função `sendEmail`.

```
Argument of type 'string | null' is not assignable to parameter of type 'string'.
  Type 'null' is not assignable to type 'string'.
```

Existem algumas formas de resolver isso, no entanto, nenhuma delas é uma boa prática.

```tsx
sendEmail(user.email || '');
sendEmail((user as any).email);
```

Ao adicionar o tipo `User` a nossa `const user`, o TypeScript agora entende que email pode ser `null`,  mesmo que nesse caso de código, esteja óbvio que o e-mail possui uma string válida. 
E é isso que o novo operador `satisfies` resolve.

```tsx
function sendEmail(email: string) {}

type User = {
  name: string;
  email: string | null;
}

const user = {
  name: "john",
  email: 'john@dev.com'
} satisfies User

sendEmail(user.email)
```

Através dele conseguimos informar para o `TypeScript` qual é a tipagem que a nossa variável precisa respeitar, mas, ao contrário de quando usamos o tipo, não perdemos a maneira padrão que o `TypeScript` inferiu a tipagem.

Vale ressaltar, que também é possível utilizar o `as const` junto com o `satisfies`, assim conseguimos ter o tipo mais específico possível pra cada propreidade, e, ao mesmo tempo, também conseguimos validar que a variável respeita a uma tipagem específica.

```tsx
const user = {
  name: "john",
  email: 'john@dev.com'
} as const satisfies User

/*
const user: {
    readonly name: "john";
    readonly email: "john@dev.com";
}
*/
```