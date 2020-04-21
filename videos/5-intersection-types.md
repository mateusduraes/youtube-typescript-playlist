## Intersection types

Combina múltiplos tipos em apenas um.

```typescript
interface Person {
  name: string;
  age: number;
}

type Loggable = {
  log: (msg: string) => void;
};

type PersonLoggable = Person & Loggable;

const person: PersonLoggable = {
  name: 'Joao',
  age: 25,
  log: (msg) => console.log(msg),
};
```

Existe um padrāo no JavaScript chamado (mixins)[https://alligator.io/js/using-js-mixins/], que se trata basicamente de unir as propriedades de dois ou mais objetos em apenas um objeto.
Os intersection types podem ser utilizados para que o resultado dessa operaçāo traga todos os tipos.

```typescript
interface Person = {
  name: 'Mateus',
  age: 25,
}


```

#TODO: Finish mixin example
