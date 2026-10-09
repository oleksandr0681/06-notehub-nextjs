# 06-notehub-nextjs

**NoteHub** — a note-taking app built with **Next.js** (App Router), **React**, and **TypeScript**. It is a homework project from the [GoIT](https://goit.global/) course and a migration of the previous Vite-based NoteHub to Next.js, adding routing, server-side data prefetching, and a note details page.

## 📋 About

Users can create, browse, search, view, and delete notes stored on the GoIT NoteHub API:

- **Home page (`/`)** — a welcome page describing the app.
- **Notes page (`/notes`)** — a list of notes (12 per page) with a debounced search, pagination, and a **Create note +** button. The first page of notes is prefetched on the server with TanStack Query (`prefetchQuery`) and passed to the client through `HydrationBoundary`, so the list is rendered without an initial loading state.
- **Note details page (`/notes/[id]`)** — a dynamic route that prefetches a single note on the server and shows its title, tag, content, and creation date. Each card in the list has a **View details** link to it.
- **Search** — the query updates one second after the user stops typing ([use-debounce](https://github.com/xnimorz/use-debounce)) and resets the page to 1. A toast is shown when nothing is found.
- **Pagination** — [react-paginate](https://github.com/AdeleD/react-paginate) appears when there is more than one page; `keepPreviousData` keeps the previous page visible while the next one loads.
- **Creating notes** — a modal (rendered with `createPortal`) with a [Formik](https://formik.org/) form validated by [Yup](https://github.com/jquense/yup): the title is required (3–50 characters, no leading or trailing spaces), the content is up to 500 characters, and the tag is one of `Todo`, `Work`, `Personal`, `Meeting`, `Shopping`. The notes list is refreshed after success.
- **Deleting notes** — a **Delete** button on each card; the list is refetched afterwards.
- **Error and loading states** — Next.js `error.tsx` and `loading.tsx` files handle failures and loading at the app, `/notes`, and `/notes/[id]` levels, and request errors are also shown as toast messages ([react-hot-toast](https://react-hot-toast.com/)).
- **Shared layout** — a header with navigation and a footer wrap every page, and the TanStack Query provider (with Devtools) is mounted in the root layout.

## 🛠️ Tech Stack

- [Next.js](https://nextjs.org/) 16 (App Router, server and client components, dynamic routes)
- [React](https://react.dev/) 19 with [React Compiler](https://react.dev/learn/react-compiler) enabled in `next.config.ts`
- [TypeScript](https://www.typescriptlang.org/)
- [TanStack Query](https://tanstack.com/query) v5 with Devtools — data fetching, SSR hydration, mutations, caching
- [Formik](https://formik.org/) and [Yup](https://github.com/jquense/yup) — forms and validation
- [axios](https://axios-http.com/) — HTTP client
- [react-paginate](https://github.com/AdeleD/react-paginate), [use-debounce](https://github.com/xnimorz/use-debounce), [react-hot-toast](https://react-hot-toast.com/), [react-spinners](https://github.com/davidhu2000/react-spinners)
- CSS Modules for component styles
- ESLint (`eslint-config-next`) and Prettier

## 📁 Project Structure

```
06-notehub-nextjs-main/
├── app/
│   ├── layout.tsx              # Root layout: header, footer, TanStack provider
│   ├── page.tsx                 # Home page
│   ├── error.tsx / loading.tsx   # App-level error and loading UI
│   └── notes/
│       ├── page.tsx               # Server component: prefetches notes
│       ├── Notes.client.tsx        # Client component: search, pagination, modal
│       ├── error.tsx
│       └── [id]/
│           ├── page.tsx              # Server component: prefetches one note
│           ├── NoteDetails.client.tsx # Client component: note details
│           └── error.tsx
├── components/
│   ├── Header/, Footer/             # Layout
│   ├── NoteList/, NoteForm/          # Cards, delete, create form
│   ├── SearchBox/, Pagination/        # Search and paging
│   ├── Modal/, Loader/                 # Modal window and loading indicator
│   └── TanStackProvider/                # QueryClientProvider + Devtools
├── lib/
│   └── api.ts                          # NoteHub API requests
├── types/
│   └── note.ts                          # Note types
├── public/                               # Static files
├── next.config.ts
└── package.json
```

## 🚀 Getting Started

Requires an LTS version of [Node.js](https://nodejs.org/) and a NoteHub API token (issued as part of the GoIT course).

1. Install dependencies:

   ```bash
   npm install
   ```

2. Create a `.env.local` file in the project root and add your token:

   ```
   NEXT_PUBLIC_NOTEHUB_TOKEN=your_notehub_token
   ```

3. Start the dev server and open [http://localhost:3000](http://localhost:3000):

   ```bash
   npm run dev
   ```

Other scripts:

```bash
npm run lint      # Lint the code
npm run build     # Create a production build
npm run start     # Run the production build
```

> **Note:** Variables prefixed with `NEXT_PUBLIC_` are embedded into the client bundle, so the token is visible in the browser. Never commit your `.env.local` file (`.env*` is already in `.gitignore`).
