Vamos ver algumas curiosidades sobre os Enums em TypeScript e consequentemente entender os motivos para evitá-los.

### Maneira mais simples de usar Enum (Enums Numéricos)

Vamos começar pela maneira mais simples de usar um enum e começamos a ver aí as curiosidades.

```tsx
enum UserType {
	ADMIN,
	REGULAR,
	PREMIUM,
}

/*
const UserType = {
  ADMIN = 0,
  REGULAR = 1
  PREMIUM = 2
}
*/

console.log(UserType);

type UserTypeUnion = 'admin' | 'regular' | 'premium'
```

- O `enum` vive no nivel da tipagem e também no nível da execução.
  - Repare que o `enum UserType` vira um objeto no nível da tipagem, que é preenchido ali de forma dinâmica através de uma função imediatamente invocada
  - Repare que o `type UserTypeUnion` não gera nada no nosso JavaScript, e é o que normalmente se espera quando está usando TypeScript.
- Como o auto incremento começa de zero `0`, se colocamos este valor num `if` , esse `if` nunca será executado.

```tsx
declare function checkUserType(userType: UserType): void

checkUserType(UserType.ADMIN);
checkUserType(0); // Not safe
checkUserType(79); // Not safe
```

- Quando usamos o enum desta maneira mais simplista, podemos passar qualquer número. Com isso acabamos perdendo a segurança em nossa tipagem.

```tsx
enum UserType {
	ADMIN = 2,
	REGULAR,
	PREMIUM,
}
```

- Uma outra curiosidade é que podemos adicionar um valor inicial e ele fará o auto incremento a partir dali.

```tsx
enum UserType {
	ADMIN = 2,
	REGULAR,
	PREMIUM = 7,
	SUPER_PREMIUM 
}
```

- Se declararmos dois `enums` com o mesmo nome, eles serão merjados de certa forma, mas com os enums numéricos podemos ter um resultado inesperado.

```tsx
enum UserType {
  ADMIN = 0,
	REGULAR,
	PREMIUM,
}

enum UserType {
  SUPER_PREMIUM
}

console.log(UserType);
/*
{
  "0": "SUPER_PREMIUM",
  "1": "REGULAR",
  "2": "PREMIUM",
  "ADMIN": 0,
  "REGULAR": 1,
  "PREMIUM": 2,
  "SUPER_PREMIUM": 0
}
*/
```

### Enums (com strings)

```tsx
enum UserType {
  ADMIN = 'admin',
	REGULAR = 'regular',
	PREMIUM = 'premium'
}

function checkUserType(userType: UserType): void {
  console.log(userType);
}

checkUserType(UserType.ADMIN);

```

- Repare que com essa opção continua existindo um enum no nivel da tipagem e outro no nivel do JavaScript

```tsx
checkUserType(UserType.ADMIN);
checkUserType('admin') // Error
```

- Ao contrário do enum numérico, em que conseguíamos passar um número `checkUserType(0)`, agora temos que sempre referenciar o `UserType` enum
- Se tratando da criação de uma lib, você torna a sua API menos flexível, pois o usuário da lib sempre terá de importar e utilizar o seu `enum`.
- Se você tem alguma função mais dinâmica, ou recebe input do usuário, você vai acabar tendo que fazer o `cast`.


### Const enums

```tsx
const enum UserType {
	ADMIN = 'ADMIN',
	REGULAR = 'REGULAR',
	PREMIUM = 'PREMIUIM'
}

const user: UserType = UserType.ADMIN;
const user2: UserType = 'ADMIN'
```

Apenas mencionar a documentação

- Imports, uso de libs, etc.
- link descrição

### Const assertions

```tsx
const userType = {
	ADMIN: 'ADMIN',
	REGULAR: 'REGULAR',
	PREMIUM: 'PREMIUIM'
} as const;

userType
// ^?

type UserType = typeof userType;
type UserType = typeof userType[keyof typeof userType]
```
