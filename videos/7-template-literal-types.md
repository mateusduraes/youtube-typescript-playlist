## Utility Types

Se você trabalha com TypeScript, provavelmente já utilizou algum `literal type.`

```tsx
const userType: string = 'admin';
// Not realy useful 👇
const userType: 'admin' = 'admin;'

```

**Literal types with unions**

No entanto, quando usamos os `literal types` em conjunto com `unions`, já atingimos um nível de controle mais interessante.

```tsx

const userType: 'admin' | 'user' | 'manager' = 'admin;'
const department: 'financial' | 'business' = 'financial'
const userTypeWithDepartment: `${'admin' | 'user' | 'manager'}_${'financial' | 'business' | 'financial'}` = 'admin_financial';

type UserType = 'admin' | 'user' | 'manager';
type Department = 'financial' | 'business'
const userTypeWithDepartment: `${UserType}_${Department}` = 'admin_financial';
```

**Template Literal Types**

Agora que já lembramos do que se trata um `literal type`, podemos dar uma olhada no `template literal type`, esta é uma funcionalidade talvez não tão utilizada no TypeScript, mas que pode ser muito útil em alguns cenários!

```tsx
const email: `${string}@{string}.${string}` = 'joaozinho@gmail.com'

```

Claro que para esse tipo de exemplo, talvez apenas a tipagem não seja suficiente e uma opção melhor seria ter uma expressão regular no seu código, assim você pode validar que um email realmente é um email, mas, sempre que você conseguir abstrair algo do seu JavaScript para a sua tipagem, isso pode ser bem interessante, pois estamos falando de menos código e menos etapas para serem executadas no browser.

Lembra daquele exemplo anterior? Onde tinhamos a função `showBox()`

Imagine que precisamos adicionar agora uma `classe css` cujo o nome seria diferente de acordo com o parametro `position`. Bom, esse pode ser um exemplo legal para um `template literal type`.

```tsx
type Position = 'top' | 'bottom' | 'right' | 'left';
showBox(position: Position, text: string) {
	 const boxClass = getBoxCssClass(position);
   return <div className=`box ${boxClass}`>I'm a box<div>
}

getBoxCssClass(position: Position): `box__${Position}` {
  return `box__${position}`
}
// "box__top" | "box__bottom" | "box__right" | "box__left"
```

