# Energy Grappling

```
client/   React + TypeScript + Vite frontend  (npm run dev -> http://localhost:3000)
server/   FastAPI + JWT + SQLite API           (python -m uvicorn app.main:app --reload -> http://127.0.0.1:8000)
```

The frontend and API are started separately. The registration and login forms call
`http://localhost:8000/api/auth/register` and `http://localhost:8000/api/auth/login` in development.

## Deploying the frontend (Cloudflare Workers)

```powershell
cd client
npx wrangler login   # one time, opens the browser
npm run build        # type-check + build into dist/ (uses .env.production)
npm run deploy       # upload dist/ to the "energygrappling" Worker
```

The site is served at `energygrappling.com` and `www.energygrappling.com` (see `client/wrangler.jsonc`).

## FastAPI backend

```powershell
cd server
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --reload --port 8000
```

Set a unique `JWT_SECRET` environment variable before deploying. User accounts are stored locally
in `server/energy_grappling.db` and passwords are stored only as PBKDF2 hashes.

The available authentication endpoints are:

- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/me` with `Authorization: Bearer <token>`

## React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type-aware lint rules:

```js
export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...

      // Remove tseslint.configs.recommended and replace with this
      tseslint.configs.recommendedTypeChecked,
      // Alternatively, use this for stricter rules
      tseslint.configs.strictTypeChecked,
      // Optionally, add this for stylistic rules
      tseslint.configs.stylisticTypeChecked,

      // Other configs...
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```

You can also install [eslint-plugin-react-x](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-x) and [eslint-plugin-react-dom](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-dom) for React-specific lint rules:

```js
// eslint.config.js
import reactX from 'eslint-plugin-react-x'
import reactDom from 'eslint-plugin-react-dom'

export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...
      // Enable lint rules for React
      reactX.configs['recommended-typescript'],
      // Enable lint rules for React DOM
      reactDom.configs.recommended,
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```
