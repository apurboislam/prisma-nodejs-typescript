# Node.js + Prisma + TypeScript (MariaDB) Boilerplate

A professional starter template for setting up **Prisma ORM** with **TypeScript** and **MariaDB** in a Node.js environment. This setup uses modern ESM (`nodenext`) and the specialized MariaDB adapter.

## 🚀 Getting Started

Follow these steps to initialize the project from scratch.

### 1. Project Initialization

Create your project directory and initialize npm.

```bash
mkdir prisma-test
cd prisma-test
npm init -y

```

### 2. Install Dependencies

Install core production packages:

```bash
npm install @prisma/client @prisma/adapter-mariadb dotenv

```

Install development tools:

```bash
npm install prisma @types/node nodemon tsx typescript --save-dev

```

### 3. TypeScript Configuration

Initialize TypeScript and update `tsconfig.json` to support modern ESM modules.

**Command:**

```bash
npx tsc --init

```

**Updated `tsconfig.json`:**

```json
{
  "compilerOptions": {
    "module": "nodenext",
    "moduleResolution": "nodenext",
    "target": "esnext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": [
    "src/**/*",
    "prisma/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist"
  ]
}

```

### 4. Package Configuration

Update `package.json` to enable ES Modules and define scripts.

1. Set `"type": "module"`.
2. Add the following scripts:

```json
"scripts": {
  "dev": "nodemon",
  "build": "tsc",
  "start": "node dist/index.js"
}

```

### 5. Nodemon Configuration

Create a `nodemon.json` file in the root directory to automate the development workflow using `tsx`.

**File:** `nodemon.json`

```json
{
    "watch": [
        "src/**/*",
        "prisma/**/*"
    ],
    "ext": "ts",
    "ignore": [
        "node_modules/**/*",
        "build/**/*"
    ],
    "exec": "tsx src/index.ts"
}

```

### 6. Prisma Initialization

Initialize Prisma with MySQL as the provider and set a custom output path.

```bash
npx prisma init --datasource-provider mysql --output ./src/generated/prisma

```

### 7. Environment & Database Connection

Update your `.env` file with your credentials.

**Standard Connection:**

```env
DATABASE_URL="mysql://USER:PASS@HOST:PORT/DATABASE"

```

**Connection with SSL:**

```env
DATABASE_URL="mysql://USER:PASS@HOST:PORT/DATABASE?ssl-ca=assets/certs/ca-certificate.crt&ssl-mode=REQUIRED"

```

> [!IMPORTANT]
> Ensure the CA certificate is placed in the correct file path as specified in your URL string.

### 8. Syncing Database Schema

Pull your existing database schema and generate the Prisma Client.

```bash
npx prisma db pull
npx prisma db generate

```

### 9. Prisma Client Instance

Create a configuration file to initialize the Prisma Client with the MariaDB adapter.

**File:** `src/config/prisma.ts`

```typescript
import 'dotenv/config';
import { PrismaMariaDb } from '@prisma/adapter-mariadb';
import { PrismaClient } from '../generated/prisma/index.js';

const adapter = new PrismaMariaDb(process.env.DATABASE_URL!);

export const prisma = new PrismaClient({ adapter });

```

### 10. Usage

Start your development server with `npm run dev`.

**File:** `src/index.ts`

```typescript
import { prisma } from "./config/prisma.js";

async function main() {
  // Use the prisma client as usual
  // const users = await prisma.user.findMany();
}

main().catch(console.error);

```

---

## 🛠️ Tech Stack

* **Runtime:** Node.js (ESM)
* **Language:** TypeScript
* **ORM:** Prisma
* **Database:** MariaDB
* **Development:** Nodemon, tsx
