### **O que é um conditional type?**

O conditional type nada mais é do que uma espécie de `if` ou `operador ternário` dentro do TypeScript.
Significa que, dado uma entrada, a tipagem resolverá para um valor ou outro.

```tsx
type IsNumber<T> = T extends number ? true : false;
type A = IsNumber<3>; // true
type B = IsNumber<number>; // false
type C = IsNumber<string>; // fals

```

### **Constraint**

Agora que já entendemos qual a sintaxe de um conditional type, vamos entender a diferença entre usar uma sintaxe de condição (conditional type) e uma sintaxe de restrição (constraint)

```tsx
type ExtractMessage<T> = T['message'];
```

Não é possível acessar da maneira acima, pois o TypeScript não possui nenhuma garantia de que o tipo passado `T` é um objeto que possui a propriedade `message`.

Uma maneira de fazer com que o `TypeScript` tenha essa garantia ou certeza, é através de um constraint

```tsx
type ExtractMessage<T extends { message: unknown }> = T['message'];

type Email = {
   author: string;
   message: string;
}

type EmailMessage = ExtractMessage<Email>; // string
```

Portanto, o type `EmailMessage` está sendo resolvido para `string` , pois a tipagem está sendo extraída da propriedade `message`.
Isso foi atingido através do uso da constraint, o tipo `ExtractMessage` agora só aceitará como parâmetro tipagens que possuam uma propriedade `message`.

Uma forma de deixar isso mais aberto, seria se livrando da `constraint` e utilizando um `conditional type`

```tsx
// type ExtractMessage<T extends { message: unknown }> = T['message'];
type ExtractMessage<T> = T extends { message: unknown } ? T['message'] : never;

type Email = {
   author: string;
}

type EmailMessage = ExtractMessage<Email>; // never
```

### Infer
****

Bom, agora já temos um entendimento do que é um conditional type e também da diferença do uso da palavra `extends` como forma de `constraint` ou de `conditional type`.

Vamos dar um passo adiante e falar do uso do `infer`.

O `infer` vai ser utilizado para que o TypeScript consiga inferir o tipo de uma variável dentro de um conditional type.

No exemplo anterior, uma outra opção que teríamos era o uso do `infer`.

```tsx
// type ExtractMessage<T extends { message: unknown }> = T['message'];
type ExtractMessage<T> = T extends { message: infer MessageType } ? MessageType : never;

type Email = {
   author: string;
}

type EmailMessage = ExtractMessage<Email>; // never

```

No entanto, como ja temos acesso através da sintaxe `T['message']` , então ele não se faz necessário.
Portanto, vejamos um exemplo onde o uso do `infer` seria a única forma de acessar o tipo desejado.

```tsx
function hello(name: string) {
  return `hello ${name}`;
}

type HelloFn = typeof hello; 
// (name: string, surname: string) => string
```

```tsx

type FnReturnType<T> = T;

// function hello...

type HelloReturn = FnReturnType<typeof hello>;
// (name: string, surname: string) => string

```

Agora, basta alterar o `FnReturnType<T>` para termos o comportamento desejado

```tsx
type FnReturnType<T> = T extends () => 'tipo de retorno' ? 'tipo de retorno' : never;
type FnReturnType<T> = T extends (arg1: string) => infer TipoDeRetorno ? TipoDeRetorno : never;

type HelloReturn = FnReturnType<typeof hello>; // string
```

No entanto, se alterarmos a função e adicionar mais um parâmetro, como a nossa tipagem só aguarda 1 parametro, a tipagem para de funcionar novamente.

```tsx
type FnReturnType<T> = T extends (arg1: string) => infer TipoDeRetorno ? TipoDeRetorno : never;

function hello(name: string, surname: string) {
  return `hello ${name} ${surname}`;
}

type HelloReturn = FnReturnType<typeof hello>; // never
```

```tsx
type FnReturnType<T> = T extends (arg1: string, arg2: string) => infer TipoDeRetorno ? TipoDeRetorno : never;
// Fixes the issue, not dynamically
type FnReturnType<T> = T extends (...args: any) => infer R ? R : never;
```

Acabamos de entender como o `infer` funciona, o legal é que o tipo criado já existe, e junto dele existem vários outros tipos também criados pelo TypeScript.

```tsx
function hello(name: string, surname: string) {
  return `hello ${name} ${surname}`;
}

type HelloReturn = ReturnType<typeof hello>;
```