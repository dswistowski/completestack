# First iteration: Minimum ~~Viable~~ Deployable Product

For hobby project, I like to define Minimum Deployable Product. 
It should be something that is easy to produce, and easy to deploy.

It's a starting point, and it's not a final product. For MDP I don't care about
the quality of the code, I don't care about the quality of the product, I don't
care about the quality of the documentation. I just want to have something that
I can deploy and show to the world.

MDP should focus on the most important features, and do not do anything else.

## MDP for this book

For this book, I want to have a simple web application that will allow me to
write blog posts. ![MDP](../images/chapter-01/mdp.png)

Functional requirements for blog site are:
1. User can write a blog post
2. User can see a list of blog posts
3. User can see a blog post    

All blog post are stored in a database. The body of blog is just a plain text.
There is no authorization.

But there are some project requirements:
1. All infrastructure should be defined as code

There will be more functional and project requirements in the future, but for now 
this is enough. We should try to not change these requirements in the future.

## Choosing a technology

For frontend, I will use [React](https://reactjs.org/). There is lots of other viable choices like:
 - [Vue](https://vuejs.org/)
 - [Svelte](https://svelte.dev/)
 - [Ember](https://emberjs.com/)
 - [Preact](https://preactjs.com/)

 React is the most popular framework, and currently every frontend developer should know it.

For backend, I want to limit my choices to frameworks that require typescript. 
There is lots of good options in different languages like for example:
 - [FastAPI](https://fastapi.tiangolo.com/) (Python)
 - [Django](https://www.djangoproject.com/) (Python)
 - [Flask](https://flask.palletsprojects.com/en/1.1.x/) (Python)
 - [Spring](https://spring.io/) (Java)
 - [Rocket](https://rocket.rs/) (Rust)
 - [Gin](https://gin-gonic.com/) (Go)
 - [Echo](https://echo.labstack.com/) (Go)
 - [Phoenix](https://www.phoenixframework.org/) (Elixir)

Having backend in different language, can be a good idea,
when there is good separation between frontend and backend.

But for single man project, having everything in one language and one codebase make sense.

Modern choices for writing TypeScript backend:
 - [Remix](https://remix.run/)
 - [Next.js](https://nextjs.org/)
 - [Fresh](https://fresh.deno.dev/)

Deno is a new runtime for TypeScript, and it does good job to push node in correct direction.
I don't think that Deno is ready for production, but it's a good idea to keep an eye on it.
Because of it there is slow movement in node community with projects like [Bun](https://bun.sh/)
making TypeScript and JavaScript development more pleasant.
The choice between Remix and Next.js is not easy. Remix is a new framework, 
and because of that it might change a lot in near future. Choosing Next.js is more safe bet.

## Bootstraping the project

Starting [Next.js](https://nextjs.org/) project is easy. Just run:

```bash
npx create-next-app@latest --typescript
```

This will create a new project, the result after this command you can find in: [this repo](https://github.com/dswistowski/completestack-blog/tree/chapter-01-after-init)


> **_Alternatives:_**  It's possible to create next.js project with [create-t3-app](https://create.t3.gg/)
> I'd use it form most of my bootstraping projects, but for this book I want to go step by step.

You can start development server with:

```bash
npm run dev
```

After pointing browser to [http://localhost:3000](http://localhost:3000) you should see:

![Next.js default page](../images/chapter-01/next-default-page.png)


## Initial configuration

Starting structure is fine for small hacking, but it's easier to maintain it, if all compiled files are in src directory. 
To archieve it you can run the command:

```bash
mkdir src
mv pages src
mv styles src
```

Lets make our index page blank, remove Home module styles:

```bash
rm src/styles/Home.module.css
```

and make `src/styles/globals.css` empty.

And then remove line: 
```typescript
import '../styles/globals.css'
```
from `src/pages/_app.tsx`
and replace `src/pages/index.tsx` with:

```typescript tsx
import type { NextPage } from 'next'

const Home: NextPage = () => {
  return (
    <h1>Hello world</h1>
  )
}

export default Home
```

## Styles

I like to use [tailwind](https://tailwindcss.com/) for styling. It's easy to use, and it's easy to customize.
Tailwind is utility first css framework allowing you to rapidly build custom designs without ever leaving your HTML.
The other popular option is [chakra](https://chakra-ui.com/), [styled-components](https://styled-components.com/),
[emotion](https://emotion.sh/docs/introduction) or [stitches](https://stitches.dev/).

Some developers prefer to use css is js approach, but I think it's adds to much boilerplate code. 

To add tailwind to the project, you can run:

```bash
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
npx tailwindcss init -p
```
This commands will istall tailwind, postcss and autoprefixer, and create `tailwind.config.js` and `postcss.config.js` files.

Then you can add tailwind to `src/styles/globals.css` file:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Last step is to update `tailwind.config.js` file, configure content option with paths of all our future pages and components:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
      "./src/pages/**/*.{js,ts,jsx,tsx}",
      "./src/components/**/*.{js,ts,jsx,tsx}",
  ],
  // ...
}
```

so tailwind will remove unused styles from production build.

finally we can update `src/pages/index.tsx` to use tailwind:

```typescript tsx
import type { NextPage } from "next";

const Home: NextPage = () => {
  return <h1 className="text-4xl font-bold p-4">Hello world!</h1>;
};

export default Home;
```

On this point, you can run `npm run dev` and see the result:

![Hello world with tailwind](../images/chapter-01/hello-world-with-tailwind.png)


It's time to commit code to git, and [push it to github](https://github.com/dswistowski/completestack-blog/tree/chapter-01-add-tailwind).

