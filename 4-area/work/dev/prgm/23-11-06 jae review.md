--- 

creation date: 23-11-06 
modification date: 23-11-06 
tag: 

---

atom/Button 을 base로 사용하는 곳에서 Style을 주입해서 사용 할 것 같다. 
```tsx

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement>{
  title: string
}

export function Button({
  title,
  ...props
}: ) {
  const _Button = document.createElement('button');
  
}
```

