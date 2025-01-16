# Recommended Setup for a Magic: The Gathering App with Next.js

## 1. Create the Next.js Project
Run the following command to initialize a new Next.js app:

```bash
npx create-next-app@latest magic-app
```

---

## 2. Add TypeScript Support (Optional but Recommended)
If you want to use TypeScript for type safety and better development experience:

1. Create a `tsconfig.json` file in the root of your project:

   ```bash
   touch tsconfig.json
   ```

2. Install the required TypeScript dependencies:

   ```bash
   npm install --save-dev typescript @types/react @types/node
   ```

3. Run the development server to let Next.js automatically populate the `tsconfig.json`:

   ```bash
   npm run dev
   ```

---

## 3. Install and Configure Tailwind CSS
Tailwind CSS is a great choice for styling the application:

1. Install Tailwind CSS and its dependencies:

   ```bash
   npm install tailwindcss postcss autoprefixer
   npx tailwindcss init
   ```

2. Update the `tailwind.config.js` file to enable all paths for purge:

   ```javascript
   module.exports = {
     content: [
       './pages/**/*.{js,ts,jsx,tsx}',
       './components/**/*.{js,ts,jsx,tsx}',
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   };
   ```

3. Add Tailwind's base styles to your `globals.css` file in the `styles` folder:

   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

---

## 4. Set Up State Management
For global state management:

- Start with React Context (built-in).
- If the app grows in complexity, consider lightweight solutions like Zustand or Redux Toolkit.

---

## 5. Scryfall API Integration
Use Next.js API routes to create backend wrappers for the Scryfall API:

1. Create an API route (e.g., `/pages/api/cards.js`):

   ```javascript
   export default async function handler(req, res) {
     const { query } = req;
     const response = await fetch(`https://api.scryfall.com/cards/search?q=${query.q}`);
     const data = await response.json();
     res.status(200).json(data);
   }
   ```

2. Fetch data from this API route in the frontend:

   ```javascript
   const fetchCards = async (query) => {
     const response = await fetch(`/api/cards?q=${query}`);
     const data = await response.json();
     return data;
   };
   ```

---

## 6. Set Up Incremental Static Regeneration (ISR)
For static content like popular Magic card sets:

Use ISR to revalidate pages automatically. Example in `getStaticProps`:

```javascript
export async function getStaticProps() {
  const res = await fetch('https://api.scryfall.com/sets');
  const sets = await res.json();

  return {
    props: { sets },
    revalidate: 60, // Revalidate every 60 seconds
  };
}
```

---

## 7. Deploy the Application
Deploy the app on Vercel, which is optimized for Next.js:

1. Push your code to GitHub or another version control system.
2. Connect your repository to Vercel.
3. Vercel will automatically detect the Next.js framework and deploy the app.

---

## Additional Recommendations

### Use ESLint and Prettier for Consistent Code Style

```bash
npm install --save-dev eslint prettier
npx eslint --init
```

### Add Testing with Jest and React Testing Library

```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom
```

This setup ensures a solid foundation for building a scalable, well-structured Magic: The Gathering app.
