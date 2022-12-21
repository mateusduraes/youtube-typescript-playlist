### Const assertion

Talvez você já tenha visto algo do tipo em alguma codebase e não entendido o motivo do `as const`.

Esse `as const` é chamado de const assertion e foi introduzido no TyepScript 3.4.
Para entendê-lo melhor, embora estranho, vamos falar sobre `const x let` primeiro.

```tsx
let age = 18;
// age -> number

const age = 18;
// age -> 18 (literal type)
```

Quando usado o `let` o TypeScript infere o tipo mais amplo, `number` como no nosso exemplo.
Ao mudarmos para `const`, como uma `const` não pode ser alterada, o TypeScript traz essa mesma segurança e especifcidade pro código através de um `literal type`.

E se quisermos ter esse mesmo comportamento em um objeto? Ou até mesmo num array.

```tsx
const obj = {
  name: 'John',
  age: 23
}

// Works
obj.name = 'test'

/*
const obj: {
  name: string;
  age: number;
}
*/
```

```tsx
const obj = {
  name: 'John',
  age: 23,
} as const

// Error
obj.name = 'test' 

/*
const obj: {
  readonly name: "John";
  readonly age: 23;
}
*/
```

Quando utilizamos o `as const` estamos falando para o TypeScript inferir o tipo da maneira mais precisa possível. Sendo assim, podemos ver que cada propriedade recebe um literal type, e, a propriedade também se torna readonly.

O mesmo pode ser feito para um array.

```tsx
const numbers = [1, 2, 3]; 
numbers[0] = 3; // works
// const numbers: number[]

const numbers = [1, 2, 3] as const; 
numbers[0] = 3; // works
// const numbers: readonly [1, 2, 3]
```

Ao utilizar `as const` com um array, em vez de ser interpretado como um `number[]` o TypScript entenderá que este array é na verdade uma tupla readonly, onde cada elemento é um literal type.

Sendo assim os métodos que alteram o array, não ficarão disponíveis.

```tsx
const numbers = [1, 2, 3] as const;  
numbers.push(4)
// Error
```

Vale ressaltar que o uso de `as const` não é um cast `(as any)`, como citado anteriormente, o nome dessa feature é `const assertion.` E ela é usada para simplesmente alterar a forma que o TypeScript infere a tipagem de uma variável.

### Onde pode ser útil a utilização de `const assertion`?

Como visto anteriormente, será util sempre que quisermos ter uma tipagem mais específica e também ter as propriedades de um objeto ou array sendo resolvidas comoo `readonly`.

No caso de arrays, como ele é interpretado como a tupla, teremos em nível de tipagem a quantidade de elementos, o que também pode ser interessante conforme o exemplo abaixo.

```tsx
const latLong = [-25647, -48974] as const;
// const latLong: readonly [-25647, -48974]
function logLatLong(lat: number, long: number) {
   console.log('Meu lat ${lat} e meu ${long}')
}

logLatLong(...latLong);
```

O exemplo acima funciona, pois o TypeScript sabe que a tupla possue exatamente 2 elementos, ao remover o `as const` teremos um erro pois a variável `latLong` será resolvida como `number[]`, o que permitiria ter qualquer quantidade de elementos, inclusive nenhum.

```tsx
const latLong = [-25647, -48974];

function logLatLong(lat: number, long: number) {
   console.log('Meu lat ${lat} e meu ${long}')
}

logLatLong(...latLong);
// A spread argument must either have a tuple type or be passed to a rest parameter.(2556)
```

Um outro ponto interessante é que se tentarmos acessar a propriedade `.length` do array, ela vai ter um literal type.

```tsx
const latLong = [-25647, -48974] as const;
latLong.length;
// length: 2

const latLong = [-25647, -48974];
latLong.length;
// Array<number>.length: number
```

Um outro exemplo interessante, é que o `as const` te permite ter `enums` na sua aplicação utilizando apenas objetos JavaScript em vez de usar a sintaxe de enum do TypeScript

```tsx
const Direction = {
  TOP: 'top',
  BOTTOM: 'bottom'
} as const

/*
const Direction: {
  readonly TOP: "top";
  readonly BOTTOM: "bottom";
}
*/

Direction.TOP
Direction.BOTTOM
```

É bem comum encontrar projetos utilizando `enums` dessa forma devido a algumas limitações do `enum` oferecido pelo TypeScript.