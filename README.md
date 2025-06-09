# Movie List App

A lightweight React (Vite) application that lets you browse popular movies from **[TheMovieDB](https://www.themoviedb.org/)**, search the catalogue, and save favourites in your browser’s local storage.

---

## ✨ Features

| Feature                       | What it does                                                                                                  |
|-------------------------------|---------------------------------------------------------------------------------------------------------------|
| **Popular Movies**            | Fetches the `/movie/popular` endpoint from TMDB and renders a responsive grid of posters.                     |
| **Search**                    | Queries the `/search/movie` endpoint via a search bar with debouncing for smoother UX.                        |
| **Favourites**                | Stores liked movies in `localStorage`, persists across page reloads, and highlights them in the UI.           |
| **Global State with Context** | Wraps the whole app in a `MoviesProvider` so any component can read / write favourites without prop-drilling. |
| **Client-side Routing**       | Uses `react-router-dom@7` for Home, Details, and Favourites pages.                                            |

## 🛠️ Concepts Practised

### `useState`
*React’s basic hook for adding reactive state to function components.* Used to keep track of:
- The current **search query**.
- The list of **movies** fetched from the API.
- The **favourites** array (read from `localStorage` on start-up).

### `useEffect`
Hook for **side effects**. Key effects in this project:
1. **Initial fetch** of popular movies when Home loads.
2. **Synchronising** favourites → localStorage whenever the favourites array changes.
3. **Fetching** new results whenever the search query updates.

### Context API
`React.createContext()` + a provider component lets us share global data (favourites) without drilling props. The provider exposes:
```js
const { favourites, addToFavourites, removeFromFavourites } = useMovies();
```
so any component can call those helpers.

### API calls with Fetch
All calls live in **`src/services/api.js`**:
```js
const API_KEY  = import.meta.env.VITE_MOVIE_LIST_API_KEY;
const BASE_URL = import.meta.env.VITE_MOVIE_LIST_BASE_URL;

export const getPopularMovies = () =>
  fetch(`${BASE_URL}/movie/popular?api_key=${API_KEY}`)
    .then(r => r.json())
    .then(d => d.results);
```

### Local Storage
The favourites list is persisted via:
```js
useEffect(() => {
  localStorage.setItem('favourites', JSON.stringify(favourites));
}, [favourites]);
```

---

## 🚀 Getting Started

### 1 · Clone & install
```bash
git clone https://github.com/sahilkhatri23/movie-list.git
cd movie-list
npm install
```

### 2 · Environment variables
Create a **`.env`** file in the project root:
```env
VITE_MOVIE_LIST_API_KEY=<your-tmdb-api-key>
VITE_MOVIE_LIST_BASE_URL=https://api.themoviedb.org/3
```

### 3 · Run locally
```bash
npm run dev   # starts Vite on http://localhost:5173
```

### 4 · Build
```bash
npm run build # outputs production bundle to /dist
```

---

## 🌐 Deploying to Vercel
1. Push the repo to GitHub.
2. Import the project in **Vercel** → *Add New Project*.
3. In **Settings › Environment Variables** add the same two keys shown above.
4. Click **Deploy**.

Vercel automatically detects Vite and builds with `npm run build`.

---

## 📂 Folder Structure
```
src/
 ├─ components/        // UI atoms & molecules
 ├─ context/MoviesContext.jsx
 ├─ pages/             // Home, Details, Favourites
 ├─ services/api.js    // TMDB helpers
 ├─ App.jsx            // routes + layout
 └─ main.jsx           // entry
```

---

## 📝 License
MIT © 2025 Sahil Khatri

---

> Built as a learning project for mastering React hooks, Context, and external API integration. PRs and suggestions are welcome!
