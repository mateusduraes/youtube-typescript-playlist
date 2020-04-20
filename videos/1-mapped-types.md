## Mapped Types

#### Partial (Provido pelo próprio TypeScript)

```typescript
interface Person {
  name: string;
  age: number;
}

const person: Person = {
  name: 'Mateus',
  age: 25,
};

const personPartial: Partial<Person> = {
  name: 'Mateus',
  // Nāo precisa ter a propriedade age, todas propriedades se tornaram opcionais com o uso do Partial
};
```

#### Readonly (Provido pelo próprio TypeScript)

```typescript
interface Person {
  name: string;
  age: number;
}

const personReadonly: Readonly<Person> = {
  name: 'Mateus',
  age: 25,
};
personReadonly.age = 23; // Error, todas propriedades de personReadonly sāo readonly (nāo podem ser alteradas)
```

#### Criando seu próprio Mapped Type - Stringify

```typescript
interface Person {
  name: string;
  age: number;
}

type Stringify<T> = {
  [P in keyof T]: string;
};

const personString: Stringify<Person> = {
  name: 'Mateus',
  age: '25', // Todas propriedades se tornam string
};
```

### Stringify em nested objects

```typescript
interface Person {
  name: string;
  age: number;
  extraInformation: {
    salary: number;
    role: string;
  };
}

type Stringify<T> = {
  /*
    Conditional typing
    Pra cada propriedade irá verificar se é objeto
        * Se nāo for objeto, coloca o tipo como string
        * Se for objeto, a tipagem do objeto será mapeada pelo stringify
    Repare que se torna algo muito parecido com uma recursividade
  */
  [P in keyof T]: T[P] extends object ? Stringify<T[P]> : string;
};

const person: Person = {
  name: 'Mateus',
  age: 25, // number
  extraInformation: {
    salary: 100, // number
    role: 'Developer',
  },
};

const personStr: Stringify<Person> = {
  name: 'Mateus',
  age: '25', // string
  extraInformation: {
    salary: '100', // string
    role: 'Developer',
  },
};
```
