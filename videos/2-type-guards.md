## Type Guards

#### Primitive types

```typescript
function printType(value: number | string) {
  // Nesta linha o typescript nāo sabe se é um number ou string
  if (typeof value === 'number') {
    // Nesta linha, o typescript sabe que é um number
  } else if (typeof value === 'string') {
    // Nesta linha o typescript sabe que é uma string
  }
}
```

#### Como fazer ensinar o TypeScript a diferenciar minhas próprias interfaces?

```typescript
interface Developer {
  name: string;
  language: string;
}

interface Designer {
  name: string;
  software: string;
}

const developer: Developer = { name: 'Mateus', language: 'Python' };
const designer: Designer = { name: 'Luis', software: 'Photoshop' };

function printSkill(person: Developer | Designer): void {
  if (person.language) {
    // Expressāo invalida, TypeScript nāo sabe nesse momento qual é o tipo da variavel
    console.log(person.language); // Expressāo invalida, TypeScript nāo sabe nesse momento qual é o tipo da variavel
  } else {
    console.log(person.software); // Expressāo invalida, TypeScript nāo sabe nesse momento qual é o tipo da variavel
  }
}
```

### Primeira soluçāo

```typescript
function printSkill(person: Developer | Designer): void {
  if ((person as Developer).language) {
    console.log((person as Developer).language);
  } else {
    console.log((person as Designer).software);
  }
}

// Fazer o cast em todas as linhas é possível, mas nāo é interessante.
```

### Segunda soluçāo

```typescript
function printSkill(person: Developer | Designer): void {
  if ('language' in person) {
    // O 'in' faz uma checagem segura no objeto, verificando se existe a propriedade e retornando um boolean
    console.log(person.language); // Aqui o TypeScript sabe que é o objeto se trata de um Developer
  } else {
    console.log(person.software); // Aqui o TypeScript sabe que é o objeto se trata de um Designer
  }
}
```

### Extraindo o Type Guard para uma funçāo com o uso do "is"

```typescript
function isDeveloper(person: Developer | Designer): person is Developer {
  return 'language' in person;
}

function printSkill(person: Developer | Designer): void {
  if (isDeveloper(person)) {
    console.log(person.language); // Aqui o TypeScript sabe que é o objeto se trata de um Developer
  } else {
    console.log(person.software); // Aqui o TypeScript sabe que é o objeto se trata de um Designer
  }
}
```
