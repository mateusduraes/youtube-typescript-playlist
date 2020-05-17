## Utility Types

### Readonly

```tsx
interface User {
  name: string;
  phone: string;
  email: string;
  password: string;
  age?: number;
}
type UserReadonly = Readonly<User>;
const user: UserReadonly = {
  name: 'John',
  phone: '0800 4578',
  email: 'john@gmail.com',
  password: '123456',
  age: 21,
};
user.email = ''; // Error
```

### Required

```tsx
interface User {
  name: string;
  phone: string;
  email: string;
  password: string;
  age?: number;
}
type UserRequired = Required<User>;
const user: UserRequired = {
  name: 'John',
  phone: '0800 4578',
  email: 'john@gmail.com',
  password: '123456',
  age: 21,
};
```

### Partial

```tsx
interface User {
  name: string;
  phone: string;
  email: string;
  password: string;
  age?: number;
}
type LoginCredentails = Partial<User>;
const credentials: LoginCredentails = {
  email: '',
  password: '',
};
```

### Pick

```tsx
interface User {
  name: string;
  phone: string;
  email: string;
  password: string;
  age?: number;
}
type LoginCredentails = Pick<User, 'email' | 'password'>;
const credentials: LoginCredentails2 = {
  email: '',
  password: '',
};
```

### Omit

```tsx
interface User {
  name: string;
  phone: string;
  email: string;
  password: string;
  age?: number;
}

type LoginCredentials = Omit<User, 'phone' | 'name'>;
const credentials: LoginCredentials = {
  email: '',
  password: '',
};
```

### Record

```tsx
interface Post {
  title: string;
  subtitle: string;
}

const PostsMappedBySections = {
  sports: [
    {
      title: 'Espanha: Barcelona vence Real Madrid',
      subtitle: 'Com gols de Ronaldinho Gaúcho...',
    },
  ],
  nutrition: [
    {
      title: 'FastFood e obesidade nos Estados Unidos',
      subtitle: 'McDonalds ou BurgerKing? Essa é a...',
    },
  ],
};

type Props = 'sports' | 'nutrition';
type PostsRecord = Record<Props, Post[]>;
```
