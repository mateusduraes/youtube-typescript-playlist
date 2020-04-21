## Legibilidade com Numeric separator

### Evite

```typescript
const interval = 60000;
setTimeout(() => {
  alert('Ola mundo');
}, interval);
```

### Prefira

```typescript
const interval = 60_000;
setTimeout(() => {
  alert('Ola mundo');
}, interval);
```

O compilador irá simplesmente remover os `_`.
