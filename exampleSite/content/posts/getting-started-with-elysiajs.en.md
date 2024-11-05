---
author:
  name: "Thiago S. Teixeira"
date: 2023-10-01
linktitle: "Getting Started with ElysiaJS"
type:
- post
- posts
tags: ["elysiajs", "bun", "typescript"]
title: "Getting Started with ElysiaJS"
description: "One of the best bun web frameworks"
weight: 101
---

![Header](/static/blog/getting-started-with-elysiajs/header.png)

### Introduction

Elysia.js is a Bun web framework, focusing on performance, simplicity, and flexibility.<br>
The version used in this post is 0.7 [Stellar Stellar](https://www.youtube.com/watch?v=AAsRtnbDs-0), a tribute to Hoshimachi Suisei.<br>
For our example project, we'll create a CRUD API to store Pokémon.

### Quick start

If you don't have Bun installed:
```bash
curl https://bun.sh/install | bash
```

Create a new project; `poke-api-elysia` is our project's name:
```
bun create elysia poke-api-elysia
```

Navigate to the poke-api-elysia directory and modify the `src/index.ts`  file:
```typescript
import { Elysia } from "elysia";

const app = new Elysia().get("/", () => "Okey dokey").listen(8000); 

console.log( `🦊 Elysia is running at ${app.server?.hostname}:${app.server?.port}` );
```

Start a development server by:
```bash
bun dev
```

Now you server is running on `http://localhost:8000`.

### Database Access
For the database, we'll use MongoDB and its JavaScript connector, Mongoose.

Installing Mongoose:
```bash
bun add mongoose
```
Or you can simply use `a` as a shorthand for `add`
```bash
bun a mongoose
```

We'll use the following schema for our Pokémon.<br>
Create a file named `pokemonSchema.ts`:
```typescript
import * as mongoose from "mongoose";

const pokemonSchema = new mongoose.Schema(
  {
    name: { type: String, required: true },
    type: { type: String, required: true },
    level: { type: Number, required: true, default: 1, min: 1, max: 100 },
  },
  {
    methods: {
      cry() {
        console.log(`${this.name}!`);
      },
    },
  }
);

export type Pokemon = mongoose.InferSchemaType<typeof pokemonSchema>;
export const Pokemon = mongoose.model("pokemon", pokemonSchema);
```
This schema has attributes: name, type, and level. The level has a default value of 1, a minimum value of 1, and a maximum of 100. The methods attribute is for adding methods to the schema; in our case, we'll add the cry method.<br>
The MongoDB collection will be named pokemon, and the schema is pokemonSchema.<br>

For the database connection, using localhost with the database named `pokemonDB`:
```typescript
import mongoose from "mongoose";
await mongoose.connect("mongodb://127.0.0.1:27017/pokemonDB");
```

### Routes
Route definition is similar to [Express](https://expressjs.com/) and [Fastify](https://fastify.dev/).<br>
Its structure is `.[method name](path, callback, hook?)`.<br>
Example of a GET route:
```typescript
new Elysia()
    .get('/ping', () => 'pong')
    .listen(8000);
```

We can also create route groups with prefixes:
```typescript
app.group('/user', app => app
    .post('/sign-in', signIn)
    .post('/sign-up', signUp)
    .post('/profile', getProfile)
);
```

Example with API versioning:
```typescript
app.group('/v1', app => app
    .get('/', () => 'Using v1')
    .group('/user', app => app
        .post('/sign-in', signIn)
        .post('/sign-up', signUp)
        .post('/profile', getProfile)
    )
);
```

### Controllers
To better organize the code, we'll create a controller for Pokémon.<br>
Create a file named `pokemonController.ts`:
```typescript
import { Elysia, t } from "elysia";
import { Pokemon } from "./pokemonSchema";

export const pokemonController = new Elysia({
    prefix: "/pokemon",
  }).post(
    "/",
    async ({ body: { name, type, level } }) => {
      const pokemon = new Pokemon({
        name,
        type,
        level,
      });
      await pokemon.save();
      return pokemon;
    }
  );
```
The controller is a class that extends Elysia, and in the constructor, we pass the route prefix.<br>
The POST is for creating a new Pokémon, and the body is the request body, which will be destructured to get the attributes: name, type, and level.<br>
The function's return is the created Pokémon.<br>
```json
{
	"name": "Pikachu",
	"type": "Electric",
	"level": 10,
	"_id": "65243bedf7093d6df73bd300",
	"__v": 0
}
```
To use the controller in `index.ts`, import the controller and add it to the app.
```typescript
import { Elysia } from "elysia";
import mongoose from "mongoose";
import { pokemonController } from "./pokemonController";

await mongoose.connect("mongodb://127.0.0.1:27017/pokemonDB");
const app = new Elysia()
  .use(pokemonController)
  .get("/", () => "Okey dokey")
  .listen(8000);

console.log(
  `🦊 Elysia is running at ${app.server?.hostname}:${app.server?.port}`
);
```


### Local Schema
Elysia comes with a schema validation feature; to define the schema, import `t`.
Example:
```typescript
import { Elysia, t } from 'elysia'

new Elysia()
    .post('/mirror', ({ body }) => body, {
        body: t.Object({
            username: t.String(),
            password: t.String()
        })
    })
```

Adding the schema to the controller:
```typescript
import { Elysia, t } from "elysia";
import { Pokemon } from "./pokemonSchema";

export const pokemonController = new Elysia({
    prefix: "/pokemon",
  }).post(
    "/",
    async ({ body: { name, type, level } }) => {
      const pokemon = new Pokemon({
        name,
        type,
        level,
      });
      await pokemon.save();
      return pokemon;
    },
    {
      body: t.Object({
        name: t.String({
          minLength: 1,
        }),
        type: t.String({
          minLength: 1,
        }),
        level: t.Integer({
          minimum: 1,
          maximum: 100,
        }),
      }),
    }
  );
```
Now, Elysia validates our request body and returns an error if it doesn't match the schema.
```
Invalid body, 'name': Required property

Expected: {
  "name": ".",
  "type": ".",
  "level": 1
}

Found: {}
```

### Path Params
Now, we need to access a specific Pokémon; for this, we'll use path params.<br>
Example:
```typescript
import { Elysia } from 'elysia'

new Elysia()
    .get('/id/:id', ({ params: { id } }) => id)
    .get('/rest/*', () => 'Rest')
    .listen(8080)
```

Applying to our project, we'll create a GET and UPDATE for the Pokémon.
```typescript
.get("/:id", async ({ params: { id } }) => {
    const pokemon = await Pokemon.findById(id);
    if (!pokemon) {
      throw new Error("Pokemon not found");
    }
    return pokemon;
  })
```

```typescript
.put(
"/:id",
async ({ params: { id }, body: { name, type, level } }) => {
    let pokemon = await Pokemon.findById(id);
    if (!pokemon) {
    throw new Error("Pokemon not found");
    }
    pokemon.name = name;
    pokemon.type = type;
    pokemon.level = level;
    await pokemon.save();
    return pokemon;
},
{
    body: t.Object({
    name: t.String({
        minLength: 1,
    }),
    type: t.String({
        minLength: 1,
    }),
    level: t.Integer({
        minimum: 1,
        maximum: 100,
    }),
    }),
}
);
```

And finally, the Pokémon deletion.
```typescript
.delete("/:id", async ({ params: { id } }) => {
    const pokemon = await Pokemon.findByIdAndDelete(id);
    if (!pokemon) {
      throw new Error("Pokemon not found");
    }
    return { message: "Pokemon deleted" };
  });
```

### Conclusion
This was a brief introduction to ElysiaJS; I only covered the basic features, but it has much more to offer.<br>
You can check out the complete code for this post on [GitHub](https://github.com/teixeirazeus/poke-api-elysia).<br>
The ElysiaJS community is very active, and the framework is well-documented. You can find various plugins online that will make development even faster.<br>
If you like learning by example, like I do, check out the project's repository. [There's a section dedicated to examples](https://github.com/elysiajs/elysia/tree/main/example).<br>
And don't forget to check out the [ElysiaJS documentation](https://elysiajs.com/introduction.html).<br>

![pokemonArt](/static/blog/getting-started-with-elysiajs/pikachu.jpeg)

Don't forget to send me your ElysiaJS projects; I'd love to see what you're doing with this framework.<br>