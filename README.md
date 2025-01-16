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

## 4. Add ShaqCDN Component Library
ShaqCDN provides pre-built components to speed up frontend development:

1. Install the ShaqCDN package:

   ```bash
   npm install shaqcdn
   ```

2. Import the library and use its components in your project. For example, to use a button:

   ```javascript
   import { Button } from 'shaqcdn';

   export default function Example() {
     return (
       <Button label="Click Me" onClick={() => alert('Button clicked!')} />
     );
   }
   ```

3. Customize the components by overriding default styles in your `tailwind.config.js` or using ShaqCDN's built-in theming options. Refer to the [ShaqCDN Documentation](https://shaqcdn.com/docs) for more details.

---

## 5. Set Up State Management
For global state management:

- Start with React Context (built-in).
- If the app grows in complexity, consider lightweight solutions like Zustand or Redux Toolkit.

---

## 6. Database Integration
To handle your own database, follow these steps:

1. **Choose a Database**:
   - For relational databases, use PostgreSQL or MySQL.
   - For non-relational databases, use MongoDB.

2. **Set Up a Database Connector**:
   - Install the appropriate database client:
     ```bash
     npm install prisma
     ```

3. **Generate Models**:
   - Initialize Prisma:
     ```bash
     npx prisma init
     ```
   - Define your data models in `prisma/schema.prisma`:
     ```prisma
     model Card {
       id        Int     @id @default(autoincrement())
       name      String
       type      String
       rarity    String
       manaCost  String
     }
     ```
   - Run migrations to apply your schema to the database:
     ```bash
     npx prisma migrate dev --name init
     ```

4. **Use Database Models in API Routes**:
   - Create API routes in Next.js and use Prisma to interact with the database:
     ```javascript
     import { PrismaClient } from '@prisma/client';

     const prisma = new PrismaClient();

     export default async function handler(req, res) {
       if (req.method === 'GET') {
         const cards = await prisma.card.findMany();
         res.status(200).json(cards);
       }
     }
     ```

5. **Secure Your Environment Variables**:
   - Add database connection details in `.env`:
     ```env
     DATABASE_URL="postgresql://user:password@localhost:5432/magicdb"
     ```
   - Ensure sensitive files like `.env` are excluded from version control by adding them to `.gitignore`.

---

## 7. Scryfall API Integration
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

## 8. Set Up Incremental Static Regeneration (ISR)
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

## 9. Deploy the Application
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
