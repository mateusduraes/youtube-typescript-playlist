## Interface vs Type, qual a diferença?

### Type

Também conhecido como `Type Alias`, sāo uma forma de dar um novo nome para um tipo. Sāo muito similares às interfaces mas podem ser usados para nomear tipos primitivos, unions, tuplas **e também tipos que precisamos escrever a māo (isso é o que torna ele tāo poderoso)**

### Interface

Principal forma de tipagem no TypeScript. Contratos.

### Entāo quais as diferenças?

#### Interfaces podem ser merjadas, types nāo.

```typescript
interface Profession {
  name: string;
}

interface Profession {
  sallary: number;
}

const dev: Profession = {
  name: 'John',
  salary: 2500,
};
```

```typescript
type Profession = {
  name: string;
};

type Profession = {
  // Error, Identificador duplo, nāo é possivel fazer Merge com types.
  salary: number;
};

const dev: Profession = {
  name: 'John',
  salary: 2500,
};
```

#### Interfaces nāo podem usar o dinamismo do `key of / in`, já os Types podem.

```typescript
type Stringify<T> = {
  [P in keyof T]: string;
}


interface IStringify<T> {
  [P in keyof T]: string; // Error, interfaces nāo trabalham com computed names.
}
```

#### Só conseguimos utilizar interfaces para tipar objetos / functions, Types podem ser usados como Alias até mesmo para tipos primitivos.

```typescript
type UserEmail = string;
function sendEmail(email: UserEmail, message: string) {}
```

Isso é o mesmo que fazer

```typescript
function sendEmail(email: string, message: string) {}
```

Segundo a documentaçāo do TypeScript, por causa do principio aberto/fechado do SOLID, vocé deve sempre usar uma interface em vez de type, se possível.
