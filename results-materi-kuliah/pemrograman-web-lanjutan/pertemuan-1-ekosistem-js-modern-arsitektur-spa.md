# Pertemuan 1: Ekosistem JavaScript Modern & Arsitektur Full-Stack SPA

## 1. Learning Outcomes
Setelah pertemuan ini, mahasiswa mampu:
- Menjelaskan ekosistem JavaScript modern dan peran Node.js, npm, dan build tools
- Memahami perbedaan client-side dan server-side, serta arsitektur Single Page Application (SPA)
- Menyiapkan lingkungan pengembangan web modern dengan VS Code, Node.js, dan npm
- Memahami konsep dasar Vue.js dan Express.js dalam konteks full-stack

## 2. Pengantar: Hook

Bayangkan 10 tahun lalu, JavaScript hanya berjalan di browser. Hari ini, JavaScript mendominasi full-stack: frontend (Vue.js, React), backend (Node.js), mobile (React Native), desktop (Electron). Ekosistem JS modern adalah **revolusi** yang mengubah cara orang membuat software.

Di Tokopedia, satu engineer bisa write JavaScript di frontend, backend, dan mobile — tidak perlu switch bahasa. Itu hanya mungkin karena ekosistem JS yang kaya dan matang.

## 3. Konsep Utama

### 3.1 Evolusi JavaScript

```
2009: Node.js lahir → JS bisa run di server
2015: ES6 (ES2015) → Class, arrow function, module → Game changer!
2016-2024: Annual updates → async/await, optional chaining, nullish coalescing
Modern: TypeScript, Vite, ESLint, Prettier, Vitest → Professional tooling
```

### 3.2 Node.js vs Browser

| Aspek | Browser | Node.js |
|-------|---------|---------|
| Runtime | V8 engine di Chrome/Firefox | V8 engine standalone |
| DOM | Ada | Tidak ada |
| File System | Tidak bisa | Bisa (fs module) |
| Network | Fetch API | http, axios |
| Use case | UI interaktif | Server, CLI tools |

### 3.3 npm (Node Package Manager)

```
npm = "App Store untuk JavaScript"

Package.json = daftar dependencies (seperti Gemfile di Ruby, requirements.txt di Python)
node_modules/ = folder tempat semua library disimpan (jangan commit!)
package-lock.json = lock file untuk reproducible builds
```

### 3.4 Arsitektur Full-Stack

```
┌─────────────────────────────────────────────┐
│  Browser (Client)                           │
│  ├─ Vue.js (UI framework)                   │
│  ├─ JavaScript ES6+ (logic)                 │
│  ├─ Tailwind CSS (styling)                  │
│  └─ Axios (HTTP client)                     │
└────────┬────────────────────────────────────┘
         │ HTTP/REST/JSON
┌────────▼────────────────────────────────────┐
│  Server (Backend)                           │
│  ├─ Node.js (runtime)                       │
│  ├─ Express.js (web framework)              │
│  ├─ Database (MySQL/Postgres)               │
│  └─ ORM (Sequelize/Prisma)                  │
└─────────────────────────────────────────────┘
```

## 4. Ilustrasi & Analogi

### Traditional vs SPA

**Traditional Multi-Page (Server Render):**
- Click link → full page reload dari server
- Lambat, user experience jelek
- "Kaya pindah rumah setiap klik"

**SPA (Single Page Application):**
- Click link → fetch data saja, UI update dengan JavaScript
- Cepat, smooth, seperti native app
- "Kaya furniture berubah, tapi rumahnya tetap"

## 5. Contoh Teknis: Setup Proyek

```bash
# 1. Install Node.js dari nodejs.org (LTS)
node --version  # v20.x atau lebih baru

# 2. Create project folder
mkdir my-app
cd my-app

# 3. Initialize npm
npm init -y
# → creates package.json

# 4. Install dependencies
npm install express cors dotenv
npm install --save-dev nodemon

# 5. Create entry point (server.js)
# → Minimal Express server

# 6. Run server
npm start
```

## 6. Studi Kasus: Gojek Aplikasi Web

Gojek punya 3 aplikasi berbeda tapi shared backend:
- **Web app (Vue.js):** Untuk driver & pelanggan di browser
- **Mobile app (React Native):** Untuk iOS/Android
- **Admin dashboard (React):** Internal tools

**Semuanya** terhubung ke **satu API yang sama** (Node.js + Express). Ini adalah keuntungan full-stack JavaScript.

## 7. Referensi

- [Node.js Official Docs](https://nodejs.org)
- [npm Official Docs](https://docs.npmjs.com)
- [Express.js Docs](https://expressjs.com)
- [Vue.js 3 Docs](https://vuejs.org)
- [MDN Web Docs](https://developer.mozilla.org)

**Status:** ✅ File P1 selesai
