# Next.js Project Initialization Steps

This repository currently contains a PHP application. To migrate to Next.js with TypeScript, start by creating a new Next.js project alongside this code base (or in a separate directory).

## 1. Prepare the environment

1. Install **Node.js** (version 18 or later is recommended). You can use `nvm` to manage Node versions:
   ```bash
   nvm install 18
   nvm use 18
   ```
2. Ensure **npm** (or **yarn** if preferred) is available. Update to the latest version:
   ```bash
   npm install -g npm
   ```

## 2. Create a Next.js application

1. From the command line, run the Next.js project generator with TypeScript support:
   ```bash
   npx create-next-app@latest next-indexhibit --typescript
   cd next-indexhibit
   ```
   This will scaffold a basic project in the `next-indexhibit` folder.
2. Initialize a git repository in the new project if desired and commit the generated files.

## 3. Install additional packages

Inside the newly created project directory, install dependencies used during the migration:

```bash
npm install next react react-dom
npm install --save-dev typescript @types/react @types/node @types/react-dom

# Styling (example: Tailwind CSS)
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Data layer (example: Redis for JSON blobs)
npm install @redis/client

# Start a local Redis instance (if not using a hosted provider)
redis-server &

# Authentication (example: next-auth)
npm install next-auth

# Image manipulation for uploads
npm install sharp
```

Adjust the list of packages as you refine the project requirements (for example, you may need libraries for file uploads, form handling, or state management).

## 4. Development tasks

1. Start the development server to verify the new setup:
   ```bash
   npm run dev
   ```
2. Open `http://localhost:3000` in your browser to see the default Next.js page.
3. Commit the baseline project so subsequent migration work has a stable starting point.

**Next steps**: follow the migration plan outlined in `the-plan.md` to port functionality from the existing PHP code into the new Next.js TypeScript project.
