## Generics

Além de te possibilitar reutilizar código de uma forma mais segura em termos de tipagem, entender `Generics` também vai te possibilitar entender algumas tipagens mais compelxas que você pode encontrar no código de alguma biblioteca.

Para começar a falar de `Generics` , vamos dar uma olhada em alguns exemplos já existentes no TypeScript, coisas que o TypeScript já disponibiliza pra você usar.

Quandemos declaramos um `array` em `TypeScript` existem duas sintaxes bem comuns.

```tsx
const countries: string[] = ['🇧🇷', '🇳🇱', '🇫🇷', '🇦🇷']
const countries: Array<string> = ['🇧🇷', '🇳🇱', '🇫🇷', '🇦🇷']

const nums: number[] = [1, 2, 3, 4]
const nums: Array<number> = [1, 2, 3, 4]
```

O tipo `Array` é um exemplo de `Generic` já implementado no TypeScript.

Sempre que usamos a sintaxe `<>` estamos trabalhando com um `Generic`.
Vamos recriar a nossa própria versão do tipo `Array` para entender como isso pode ser feito por baixo dos panos.

```tsx
type MyList<T> = T[];
const countries: string[] = ['🇧🇷', '🇳🇱', '🇫🇷', '🇦🇷']
const countries: MyList<string> = ['🇧🇷', '🇳🇱', '🇫🇷', '🇦🇷']

const nums: number[] = [1, 2, 3, 4]
const nums: MyList<number> = [1, 2, 3, 4]
```

Declarar um tipo como `T` é apenas uma convenção de mercado, já que o `T` remete a `T`ype.
No entanto, podemos usar qualquer nome.

```tsx
type MyList<ElementType> = ElementType[];
const countries: string[] = ['🇧🇷', '🇳🇱', '🇫🇷', '🇦🇷']
const countries: MyList<string> = ['🇧🇷', '🇳🇱', '🇫🇷', '🇦🇷']

const nums: number[] = [1, 2, 3, 4]
const nums: MyList<number> = [1, 2, 3, 4]
```

Agora, com um entendimento do que é um tipo genérico e de como criar um, vamos aplicar este conhecimento numa função, vamos começar do começo, com algo simples, e, vamos aumentando a complexidade aos poucos.

```tsx
const obj = { name: 'jhon' };

function addLocation(obj: object, location: string) {
	 return {
      ...obj,
      location,
   }
}

const personWithLocation = addLocation(obj, 'Brazil')
/*
Typescript infers 👇
const personWithLocation: {
    location: string;
}
*/
```

Criamos uma função `addLocation` , que é responsável por adicionar um propriedade `location` em um objeto que é passado por parâmetro. Porém como o Typescript não sabe quais são as possíveis propriedades desse objeto, a única coisa que ele é capaz de inferir, é que no fim das contas, o objeto retornado terá a propriedade `location: string`.

Isso funciona, mas perdemos a tipagem do nosso objeto!

Para resolver isso, podemos adicionar uma tipagem ao objeto e dizer que a função espera o tipo `Person`. Isso resolve o problema e agora o TypeScript consegue inferir o objeto retornado corretamente.

```tsx
type Person = {
    name: string;
}

const obj = { name: 'jhon' };

function addLocation(obj: Person, location: string) {
	 return {
      ...obj,
      location,
   }
}

const personWithLocation = addLocation(obj, 'Brazil')
/*
Typescript infers 👇
const personWithLocation: {
    location: string;
    name: string;
}
*/
```

Agora recebemos um novo requisito e também precisamos adicionar `location` a objectos to tipo `Company`. Parece simples.

A primeira versão da função não estava atrelada a nenhum tipo, mas não era segura em termos de tipagem, pois perdiamos completamente a referência de qual tipo era retornado.

Uma solução que funciona, mas não é tão interessante é duplicar a função para cada caso de uso.

Mas, repare que a implementação da função é a mesma.

```tsx
type Person = {
    name: string;
}

const person: Person = { name: 'jhon' };

function addLocationToPerson(obj: Person, location: string) {
	 return {
      ...obj,
      location,
   }
}

const personWithLocation = addLocationToPerson(person, 'Brazil')

/* Typescript infers 👇
{
    location: string;
    name: string;
}
*/

// -------------------------------------------------------------------------

type Company = {
    name: string;
    business: string;
    phone: string;
}

const company: Company = {
    name: 'Facebook',
    business: 'social',
    phone: '3145124'
}

function addLocationToCompany(obj: Company, location: string) {
	 return {
      ...obj,
      location,
   }
}

const companyWithLocation = addLocationToCompany(company, 'USA')
/* Typescript infers 👇
{
    location: string;
    name: string;
    business: string;
    phone: string;
}
*/
```

Como dito anteriormente, os `Generics` te ajudam a reutilizar o código de maneira correta quando o assunto é a tipagem.
Sendo assim, vamos implementar a versão genérica da nossa função.

```tsx
type Person = {
    name: string;
}

const obj = { name: 'jhon' };

function addLocation<T>(obj: T, location: string) {
	 return {
      ...obj,
      location,
   }
}

const personWithLocation = addLocation(obj, 'Brazil')
/*
Typescript infers 👇
const personWithLocation: {
    name: string;
} & {
    location: string;
}
*/
```

Agora com o uso de `Generic` a função pode receber um parâmetro do tipo `T` e o TypeScript irá inferir o retorno corretamente. Podemos chamar a mesma função com um objeto diferente e tudo funciona normalmente.

```tsx
type Company = {
    name: string;
    business: string;
    phone: string;
}

const company: Company = {
    name: 'Facebook',
    business: 'social',
    phone: '3145124'
}

const personWithLocation = addLocation(obj, 'Brazil')
const companyWithLocation = addLocation(company, 'USA')
```

Repare que as tipagens `Company` e `Person` não estão sendo utilizadas, o TypeScript está inferindo quais são as propriedades do objeto, a partir do momento em que usamos essas tipagens, a interpretação do TypeScript já fica levemente diferente.

```tsx
const personWithLocation = addLocation<Person>(obj, 'Brazil')
const companyWithLocation = addLocation<Company>(company, 'USA')
/*
Person & {
 location: string;
}
*/

/*
Company & {
 location: string;
}
*/

```

Parece tudo ótimo, estamos usando `Generics`e estamos reutilizando o código.

Mas, o que acontece se passarmos uma `string` ou um `number` para o nossa função?

```tsx
function addLocation<T extends object>(obj: T, location: string) {
	 return {
      ...obj,
      location,
   }
}

const obj = { name: 'jhon' };
addLocation(obj, 'Brazil')
addLocation('test', 'Brazil')
addLocation(3, 'Brazil')
```

E se o `location` , em alguns casos, não for uma simples string, mas sim um conjunto de `[lat,long]`? 

```tsx
function addLocation<T extends object>(obj: T, location: string | [number, number]) {
	 return {
      ...obj,
      location,
   }
}

const obj = { name: 'jhon' };
addLocation(obj, [-423123, 254564])
```

No entanto, se as possibilidades de `location` crescem, não fica legal adicionarmos todas elas ali na assinatura da função, então vamos tornar isso um `Generic`  também

```tsx
function addLocation<T extends object, L>(obj: T, location: L) {
	 return {
      ...obj,
      location,
   }
}

const obj = { name: 'jhon' };
addLocation(obj, [-423123, 254564])
```

Ao tornar `location` um generic, essa propriedade fica aberta a receber qualquer valor, então se for o caso também podemos aplicar as restrições aqui e falar que `location` deveria ser um string ou um tuple `[number, number]` por exemplo

```tsx
type LocationT = [number, number] | string;

function addLocation<T extends object, L extends LocationT>(obj: T, location: L) {
	 return {
      ...obj,
      location,
   }
}
```