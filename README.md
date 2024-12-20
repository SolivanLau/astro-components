# Astro Components: Blog Cards

## 📝 Project Overview

This is a simple recreation project based on a [blog card design](https://www.uidesigndaily.com/posts/sketch-blog-cards-post-article-thumbnail-day-997) from [UI Design Daily](https://www.uidesigndaily.com).

**The goal:** Utilize Astro's UI approach to...

- create reusable layouts.
- create modular components to form larger components.
- map data to components.
- style components and layouts with ease.

**Tech Stack:** A bare bones techstack:

- Astro
- Scss for styling
- javascript and a little typescript for static generation

## 🍃 Quickstart

To get started, clone the repository and run the following command in the terminal to see the site in action:

```sh
pnpm dev
```

## 🚀 Project Structure

This project has the following folder and file structure:

```text
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   ├── data/
│   ├── layouts/
│   ├── pages/
│       └── index.astro
│   ├── styles/
│       ├── base/
│       └── styles.scss
└── package.json
```

## 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## 👀 Learnings?

### Slots

Slots have been powerful to insert content into components, often allowing for reusability. We can start with an **unnamed slot**, which is where we can place **default** content into the component.

```jsx
<div class="card">
	<slot />
</div>
```

```jsx
<Card>
	<h5>Astro Slots</h5>
</Card>
```

Astro slots have more complexity and functionality to allow for variations of a component.

#### slots.has()

**Documentation:** [Astro Slots](https://docs.astro.build/en/reference/astro-syntax/#astroslots)

A simple helper to check if a slot exists. This is super helpful to conditionally render wrapping content around the slot itself.

```jsx
<div class="card">
	{Astro.slots.has("title") && (
		<div class="card__title">
			<slot name="title" />
		</div>
	)}
	<div class="card__content">
		<slot />
	</div>
</div>
```

```jsx
// title is slotted in with a specific position and wrapper mark up
<Card>
	<h2 slot="title">My Title </h2>
	<p>lorem ipsum</p>
</Card>
```

### Types and Components

**Documentation:** [Built-in HTML attributes](https://docs.astro.build/en/guides/typescript/#built-in-html-attributes)

With typescript, you may notice a lot of components require you to give additional prop types to allow for basic functionality. Take a look:

```typescript
// Props for a link component
interface Props {
	href: string;
	isExternal?: boolean; // conditionally render target and rel attr based on boolean
	class?: string; // allow class attribute
	[key: string]: any; // allow any additional attributes not mentioned
}
```

Astro uses a **strict** type system and requires explicitly defined types for each component prop. Although technically correct, it's not ideal to add class and key prop types to each component.

We can leverage Built-in HTML attributes for DRY types. Import `HTMLAttributes` from the `astro/types` and extend personal props from a specific element.

```typescript
import type { HTMLAttributes } from "astro/types";
// Props for a link component
interface Props extends HTMLAttributes<"a"> {
	isExternal?: boolean; // conditionally render target and rel attr based on boolean
	// href, class, and key are already defined!
}
```

Notice how we omit `href` in addition to `class` and `key` from the component's props? That's because we are extending from an ordinary HTML `<a>` element. We only include custom properties at that point.
