---
title: Discover the Power of Seamless Communication with TypeScript Remote Procedure Call (tRPC) 
date: '2023-11-14'
tags: ['tRPC', 'typescript', 'react', 'nextjs', 'postgreSql', 'prisma', 'prisma studio']
draft: false
summary: 'Imagine a seamlessly type-safe tech stack, from database to frontend….. hold on!'
---



![Alt text](/static/images/blog/trpc/image.png)

# Why tRPC?
![Alt text](/static/images/blog/trpc/image-1.png)

We've all faced mismatches in our API data types, leading to errors and headaches.  The frontend expects a string, but the backend sends an integer. Or the database returns a date in one format, but the frontend expects it in another.
Now, 
Imagine a seamlessly type-safe tech stack, from database to frontend…


## tRPC, Prisma, and PostgreSQL
![Alt text](/static/images/blog/trpc/image-2.png)

That's our goal!!

Achieving this dream of type consistency is possible with our key players today: with, tRPC, Prisma, and PostgreSQL.


## Deep dive into tRPC


![Alt text](/static/images/blog/trpc/image-3.png)

1. It's a transformative approach to building robust APIs. Unlike traditional REST where endpoints can become numerous and unwieldy, tRPC streamlines these through procedure calls. And while GraphQL offers flexible queries, it sometimes adds complexity with resolvers and schema synchronization. 

2. With tRPC, you get the best of both worlds: simplicity and flexibility.
tRPC's native integration with TypeScript ensures automatic type inference and generation. This means the backend and frontend are always in sync. No more type mismatches, no more runtime 
surprises.

3. It's a modern solution for developers who want precision, performance, and peace of mind.

## Prisma's Role

![Alt text](/static/images/blog/trpc/image-4.png)

1. Modern ORM: Offers a fresh, type-safe approach to database interactions.

2. Type Safety: Automatically infers database schema types into TypeScript types.

3. Simple Migrations: With tools like prisma db push for development and Prisma Migrate for production.

4. Prisma Studio: A powerful GUI to view and edit your database records.

5. Direct Database Access: Write raw SQL when needed, but with type safety.

## PostgreSQL

![Alt text](/static/images/blog/trpc/image-5.png)

1. Robust and Reliable: One of the most trusted relational databases in existence.

2. Open Source: Community-driven with a vast array of plugins and extensions.

3. ACID Compliant: Ensures data validity even in non-ideal situations.

4. Extensible: Supports stored procedures, custom data types, and more.

5. Perfect Pair: Combines seamlessly with Prisma for type-safe database interactions.


## Setting Up the Stack

##### Terminal snippets:
 ***npx create-next-app my-app-typescript***

***rom install trec prisma @prisma/client***

***npx prisma init*** 

##### Prerequisites: 
- Solid foundations are essential. Begin with installing Nodes and setting up a TypeScript project. 

##### Key Packages: 
- tRPC: Enables efficient API creation within Next js apps, without needing a REST or GraphQL API.

- Prisma: Modern database access for TypeScript & Node.js, acting as our ORM.

- Additional Dependencies: Ensure all necessary tools and libraries are installed. 

##### Initialization:
- Use ***'create-next-app'*** with TypeScript template for a swift project setup. - Initialization of Prisma for database schema and management. 

##### Configuration:
 -Connect Prisma to your database: Setup a 'env' file with appropriate database credentials. - Highlight the importance of secure and accurate database connection strings.


