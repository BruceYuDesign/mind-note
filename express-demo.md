# Express + Prisma CRUD API

這是一個使用 Express 和 Prisma 的簡單 CRUD API 範例。這個範例將會使用 PostgreSQL 作為資料庫。

```bash
npm init -y
npm install express prisma @prisma/client
npx prisma init
```

```prisma [prisma/schema.prisma]
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = "file:./dev.db"
}

model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String @unique
}
```

```bash
npx prisma migrate dev --name your_change
```

```js [index.js]
const express = require("express");
const { PrismaClient } = require("@prisma/client");
const app = express();
const prisma = new PrismaClient();

app.use(express.json());

// CREATE
app.post("/users", async (req, res) => {
  const { name, email } = req.body;
  const user = await prisma.user.create({ data: { name, email } });
  res.json(user);
});

// READ
app.get("/users", async (req, res) => {
  const users = await prisma.user.findMany();
  res.json(users);
});

// READ by ID
app.get("/users/:id", async (req, res) => {
  const user = await prisma.user.findUnique({
    where: { id: parseInt(req.params.id) },
  });
  res.json(user);
});

// UPDATE
app.put("/users/:id", async (req, res) => {
  const { name, email } = req.body;
  const user = await prisma.user.update({
    where: { id: parseInt(req.params.id) },
    data: { name, email },
  });
  res.json(user);
});

// DELETE
app.delete("/users/:id", async (req, res) => {
  await prisma.user.delete({ where: { id: parseInt(req.params.id) } });
  res.json({ message: "User deleted" });
});

// 啟動伺服器
app.listen(3000, () => {
  console.log("Server is running on http://localhost:3000");
});
```