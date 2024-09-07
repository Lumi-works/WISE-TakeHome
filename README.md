# Product Management System

Frontend: Typescript & Next.js & Tailwind & NextUI
Backend: MikroORM & PostgreSQL

## Table of Contents

- [Product Management System](#product-management-system)
  - [Table of Contents](#table-of-contents)
  - [Project Setup](#project-setup)
    - [Requirements](#requirements)
    - [Installation](#installation)
  - [Folder Structure](#folder-structure)
  - [Backend Setup](#backend-setup)
    - [MikroORM Configuration](#mikroorm-configuration)
  - [Running the Project](#running-the-project)

## Project Setup

### Requirements

- Node.js (>= 18.0.0)
- PostgreSQL
- MikroORM
- Next.js (>= 13)

### Installation

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd <project-directory>
   ```

2. **Install dependencies:**

   ```bash
   npm install
   ```

3. **Set up environment variables:**

   Create a `.env.local` file in the root directory and add the following environment variables:

   ```env
   DATABASE_URL=postgresql://user:password@localhost:5432/database
   ```

   Replace `user`, `password`, and `database` with your PostgreSQL credentials.

## Folder Structure

- `src/app/api/product/route.ts`: API route for fetching products.
- `entities/Product.ts`: MikroORM entity definition for products.
- `public/`: Static assets.
- `src/components/ProductCard.tsx`: Component to display individual product cards.
- `src/app/page.tsx`: Frontend component to fetch and display products.
- `utils`: Initialize MikroORM (having some trouble setting up)

## Backend Setup

### MikroORM Configuration

1. **Create the MikroORM configuration file:**

   ```typescript
   // config/mikro-orm.ts
   import { Options, PostgreSqlDriver } from "@mikro-orm/postgresql";
   import { Product } from "../entities/Product";
   import { Cart } from "../entities/Cart";

   const config: Options = {
     driver: PostgreSqlDriver,
     migrations: {
       path: "/migrations",
     },
     entities: [Product, Cart],
     dbName: "testdb",
     user: "postgres",
     password: "psql",
     host: "localhost",
     port: 5432,
     debug: process.env.NODE_ENV !== "production",
   };

   export default config;
   ```

2. **Create the Product entity:**

   ```typescript
   // lib/database/entities/Product.ts
   import { Entity, PrimaryKey, Property } from "@mikro-orm/core";

   @Entity()
   export class Product {
     @PrimaryKey({ autoincrement: true })
     id!: number;

     @Property()
     name!: string;

     @Property()
     description!: string;

     @Property()
     price!: number;

     @Property()
     imageUrl!: string;

     constructor(
       name: string,
       description: string,
       price: number,
       imageUrl: string
     ) {
       this.name = name;
       this.description = description;
       this.price = price;
       this.imageUrl = imageUrl;
     }
   }
   ```

3. **Database Initialization and Migration:**

   Run the following commands to initialize and migrate the database:

   ```bash
   npx mikro-orm migration:generate --initial
   npx mikro-orm migration:up
   ```

## Running the Project

1. **Start the development server:**

   ```bash
   npm run dev
   ```

2. **Visit the application:**

   Open `http://localhost:3000` in your browser to view the application.
