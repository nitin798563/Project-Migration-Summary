## ✅ **Project Migration Summary: From React + Vite → Next.js (App Router)**

### 🔹 1. **Removed Vite-Specific Files**
- **What**: Deleted `vite.config.ts`, `index.html`,`App.tsx`,`main.tsx` and `dist/` folder. Merge both `index.css` and `App.css` into `global.css` and import it into `app/layout.tsx`.
   Merge both `index.html` and `App.tsx` into `layout.tsx` and  `Providers.tsx(Components)`
- **Why**: These files are only for Vite. Next.js uses its own system and does **not need them**.

- **Result**: Clean project structure for Next.js.

---

### 🔹 2. **Created Next.js Folder Structure**
- **What**:  
  - Renamed `src/pages/` → `src/app/`  
  - Each page (e.g., `About.tsx`) → moved to `app/about/page.tsx`  
  - Created `app/not-found.tsx` for 404 errors  
  - Created `app/layout.tsx` as the base layout
  - Created `app/page.tsx` for the home page by renaming the `index.tsx`

- **Why**:  
  Next.js App Router uses **file-based routing**:  
  - Folder name = URL path  
  - `page.tsx` = the page content  
  - `not-found.tsx` = automatic 404 page
- **Result**: Proper Next.js routing system.

---

### 🔹 3. **Updated `package.json`**
- **What**: Added:
  ```json
  "dependencies": {
    "next": "14.2.3",
    "react": "18",
    "react-dom": "18"
  },
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
  ```
- **Why**:  
  - Next.js needs its own CLI (`next dev`)  
  - React 19 caused conflicts → downgraded to **React 18** (stable with Next.js)
- **Result**: Project now runs with Next.js commands.

---

### 🔹 4. **Fixed `tsconfig.json`**
- **What**: Replaced old Vite config with:
  ```json
  "moduleResolution": "bundler",
  "baseUrl": ".",
  "paths": { "@/*": ["src/*"] }
  ```
- **Why**:  
  Next.js 13+ requires `"moduleResolution": "bundler"` and proper path aliases to support `@/components/...`
- **Result**: `@/assets/logo.png` and similar imports now work.

---

### 🔹 5. **Handled Images Properly**
- **What**:  
  - Option A: Imported images directly → `import logo from '@/assets/logo.png'` + used `logo.src`  
  - Option B (Recommended): Moved images to `public/images/` → used `<img src="/images/logo.png" />`
- **Why**:  
  - `src/assets/` is not public by default in Next.js  
  - `public/` folder is served as static files (like `/images/logo.png`)
- **Result**: Images now load correctly.

---

### 🔹 6. **Removed `import React from 'react'`**
- **What**: Deleted this line from all files.
- **Why**:  
  Next.js 13+ uses **automatic JSX runtime** — no need to import `React`.
- **Result**: Cleaner code, no errors.
---

### 🔹 7. **Change `these Statement`**
 - import {Link, useNavigate} from 'react-dom-router'<br>
  👇<br>
 import Link from 'next/link'<br>
 import {useRouter} from 'next/navigation'

     ```sh
   import Link from 'next/link'
   <Link href="/about"> About </Link>

     ```

```sh
  'use client'
  import {useRouter} from 'next/navigation'

  const router = useRouter()
  router.push("/login")
```

- import {useSearchParams} from 'react-router-dom'<br>
👇<br>
import {useSearchParams} from 'next/navigation'
```sh
  'use client'
  import {useSearchParams} from 'next/navigation'

  const searchParams = useSearchParams() #use direct object, not array 👉     [searchParams]❌     (in react -> array)
  #             searchParams✅     (in next -> object)
   #   //remaining code
```

- write `useclient` on top where useState, useEffect, onClick, onChange, useRouter, usePathname useSeachParams is used
---

### 🔹 8. **Fixed Browser APIs (window, location, etc.)**
- **What**:  
  - Replaced `window.location` → with `usePathname()` from `next/navigation`  
  - Added `'use client'` to components using `useState`, `useEffect`, or `window`
- **Why**:  
  Next.js Server Components **cannot access browser APIs** like `window` or `location`.  
  `'use client'` makes a component run only in the browser.
- **Result**: No more `window is not defined` errors.

---

### 🔹 9. **Fixed 404 Page (`not-found.tsx`)**
- **What**:  
  - Created `src/app/not-found.tsx` (not `NotFound.tsx` or inside a folder)  
  - Used `usePathname()` instead of `useLocation` from React Router
- **Why**:  
  Next.js has a **special file name** for 404 pages: `not-found.tsx`  
  React Router hooks don’t work in Next.js.
- **Result**: Custom 404 page now works.

---

### 🔹 10. **Added Favicon (Browser Tab Logo)**
- **What**:  
  - Placed `favicon.ico` in `public/` folder  
  - Added to `layout.tsx`:
    ```ts
    export const metadata = {
      icons: { icon: "/favicon.ico" }
    };
    ```
- **Why**:  
  Next.js looks for favicon in `public/` and needs it declared in `metadata`.
- **Result**: Logo now appears in browser tab.

---

### 🔹 11. **Installed & Fixed Third-Party Libraries**
- **What**:  
  - Installed `@radix-ui/react-accordion`, `react-google-recaptcha`, etc.  
  - Wrapped their usage in `'use client'` components
- **Why**:  
  These libraries use browser features → must be used in **Client Components**.
- **Result**: UI components (Accordion, reCAPTCHA) now work.

---

### 🔹 12. **Used `npm install --force` (Temporary Fix)**
- **What**: Ran `npm install --force` to bypass dependency conflicts.
- **Why**: React 19 was causing version conflicts with libraries like `next-themes`.
- **Note**: Later, downgraded to React 18 for long-term stability.

---

## ✅ Final Result:
✅ Your **React + Vite project is now fully converted to Next.js App Router**  
✅ All pages, images, forms, and UI components work  
✅ No more `window is not defined`, `module not found`, or `404` issues  
✅ Project follows Next.js best practices

---

## 📌 Key Rules to Remember in Next.js:
| Do This | Don’t Do This |
|--------|---------------|
| Put static files in `public/` | Use `src/assets/` in `<img src="...">` |
| Use `'use client'` for interactive components | Use `window`, `localStorage` in Server Components |
| Name 404 page → `not-found.tsx` | Name it `NotFound.tsx` or put in a folder |
| Use `usePathname()` instead of `window.location` | Use React Router hooks |
| Keep `favicon.ico` in `public/` | Expect it to work from `src/` |
