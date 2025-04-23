# Workshop Lab: Bygg en Next.js App med App Router

**Mål**

Ni skapar en enkel Next.js applikation som använder App Router, `next/font`, `next/image`, `next/link` och komponenter med CSS Modules, samt anropar ett valfritt publikt API.

---

:books: Dokumentation:

<https://nextjs.org/docs>

---

## 1. Förberedelser

1. Installera Node.js (v18+).
2. Skapa och starta projektet:
   ```bash
   npx create-next-app@latest
   cd [namn på ditt projekt]
   npm run dev
   ```

Projektet körs nu på <http://localhost:3000>.

---

## 2. Projektstruktur

```bash
my-lab-app/
├── app/
│   ├── layout.tsx          # Global layout
│   ├── page.tsx            # Rot (/)
│   ├── head.tsx            # Metadata-inställningar
│   ├── api/
│   │   └── data/route.ts   # API-route
│   └── posts/
│       └── page.tsx        # Exempel på nested route
├── components/
│   ├── atoms/
│   │   └── Button/
│   │       ├── Button.tsx
│   │       └── Button.module.css
│   └── molecules/
│       └── Card/
│           ├── Card.tsx
│           └── Card.module.css
├── public/                 # Statisk media (bilder, ikoner)
├── styles/                 # Globala CSS (t.ex. globals.css)
├── next.config.js
├── tsconfig.json
└── package.json
```

---

## 3. next/font

Importera och använd en Google-font (t.ex. Inter) i `app/layout.tsx`:

```ts
import { Inter } from 'next/font/google'
const inter = Inter({ subsets: ['latin'], display: 'swap' })

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="sv" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

---

## 4. next/image

Optimera bilder med komponenten `<Image>`:

1. Lägg en fil `sample.jpg` i `public/`.
2. I `app/page.tsx`:

```tsx
import Image from 'next/image'
import sample from '/public/sample.jpg'

export default function HomePage() {
  return (
    <main>
      <h1>Välkommen</h1>
      <Image src={sample} width={600} height={400} alt="Exempelbild" />
    </main>
  )
}
```

---

## 5. next/link

Lägg till navigering utan full omladdning:

- Skapa `app/posts/page.tsx`:

```tsx
import Link from 'next/link'

export default function PostsPage() {
  return (
    <ul>
      <li><Link href="/posts/1">Post 1</Link></li>
      <li><Link href="/posts/2">Post 2</Link></li>
    </ul>
  )
}
```

- Lägg till `<Link>` i din meny i `layout.tsx`.

---

## 6. Komponenter med CSS Modules

### Atom: Button

```tsx
// components/atoms/Button/Button.tsx
i mport styles from './Button.module.css'
export default function Button({ children, onClick }: { children: React.ReactNode; onClick?: () => void }) {
  return (
    <button className={styles.button} onClick={onClick}>
      {children}
    </button>
  )
}
```

```css
/* components/atoms/Button/Button.module.css */
.button {
...
}
```

### Molecule: Card

```tsx
// components/molecules/Card/Card.tsx
import styles from './Card.module.css'
export default function Card({ title, children }: { title: string; children: React.ReactNode }) {
  return (
    <div className={styles.card}>
      <h2>{title}</h2>
      <div>{children}</div>
    </div>
  )
}
```

```css
/* components/molecules/Card/Card.module.css */
.card {
...
}
```
---
## 7. Lägg till dina komponenter i dina pages

1. Importera och använd dina atoms och molecules i app/page.tsx eller andra rutter.

Exempel:
```ts
import Button from '@/components/atoms/Button/Button'
import Card from '@/components/molecules/Card/Card'

export default function HomePage() {
  return (
    <main>
      <Card title="Hej!">
        Här är en dynamisk Card-komponent.
        <Button onClick={() => alert('Hej')}>Klicka mig!</Button>
      </Card>
    </main>
  )
}
```

2. Inspektera DOM i webbläsaren: kontrollera att dina CSS Module‐klasser är genererade med unika ID:n (t.ex. Button_button__abc123).
   
---

## 8. Anropa ett publikt API via app/api

1. Skapa `app/api/data/route.ts`:

Exempelvis:

```ts
import { NextResponse } from 'next/server'

export async function GET() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5')
  const posts = await res.json()
  return NextResponse.json(posts)
}
```

2. Hämta data i `app/posts/page.tsx`:

```tsx
import Card from '@/components/molecules/Card/Card'

export default async function PostsPage() {
  const res = await fetch('/api/data')
  const posts = await res.json()
  return (
    <div>
      {posts.map((p: any) => (
        <Card key={p.id} title={p.title}>
          {p.body}
        </Card>
      ))}
    </div>
  )
}
```

---

## 9. Kör och testa

1. Verifiera att:
   - Google-fonten laddas globalt.
   - Bilder optimeras via `<Image>`.
   - Navigering sker med `<Link>` utan omladdning.
   - Komponenter använder sina CSS Modules.
   - Data returneras från din egen `/api/data`.

**Lycka till och ha så kul!**

