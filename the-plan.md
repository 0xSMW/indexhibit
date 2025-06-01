# Migration Plan: PHP to Next.js (TypeScript)

This document outlines tasks required to migrate the existing PHP-based Indexhibit project to a modern Next.js application written in TypeScript.

---

## 1. Analyze the current PHP codebase

1. Examine entry points such as `index.php` to understand how configuration, routing, and plugins are loaded. For example, the current root script loads configuration files and initializes hooks:
   ```php
   if (file_exists('ndxzsite/config/config.php')) require_once 'ndxzsite/config/config.php';
   require_once 'ndxzsite/config/options.php';
   require_once 'ndxzstudio/defaults.php';
   require_once 'ndxzstudio/common.php';
   $OBJ =& load_class('core', true, 'lib');
   $OBJ->assign_core_front();
   ```
2. Review templates such as `ndxzsite/default/index.php` to map how pages are composed. These templates use custom tags (`<last:...>` and `<plugin:...>`) that will need React/Next.js equivalents.
3. Inspect the installation script (`ndxzstudio/install.php`) to determine the database schema—tables like `objects`, `media`, `sections`, `settings`, and `plugins` are created here.
4. Catalog existing assets (images, CSS, JavaScript) under `ndxzsite` and `ndxzstudio/asset` for migration into the Next.js project.

## 2. Plan the new Next.js architecture

1. **TypeScript**: the project will be written entirely in TypeScript for type safety.
2. **Routing**: use Next.js file-based routing to replace PHP routes. Dynamic routes will mirror how `index.php` resolves content based on URL parameters.
3. **API routes**: create Next.js API endpoints for data operations (CRUD for exhibits, media, users, etc.). These will replace the various PHP modules found in `ndxzstudio/module`.
4. **Data layer**: instead of a traditional relational database, consider a document or key‑value store. Because this project data is mostly non‑transactional, you can store JSON blobs in a datastore like **Redis** (via a library such as `@redis/client`) or another NoSQL solution. Models that were formerly tables (`objects`, `media`, `sections`, etc.) become serialized JSON objects.
5. **Authentication**: implement a modern auth solution (e.g., `next-auth`) to handle login for the admin dashboard.
6. **Admin dashboard**: re‑create the functionality of `ndxzstudio` as protected routes/pages in Next.js. Each module (exhibits, system settings, user management, statistics, etc.) becomes a React component or page under `/admin`.
7. **Front‑end rendering**: convert templates from `ndxzsite/default` and any other themes into React components. Replace `<last:...>` and `<plugin:...>` tags with appropriate React logic or custom components.
8. **File uploads & media**: use Next.js API routes combined with a library like `multer` or `next-cloudinary` to upload files. Store metadata in the `media` table and files under `/public` or an external storage service.
9. **Plugin system**: replicate the hook mechanism (see `ndxzstudio/lib/hook.php`) with a plugin architecture in Node/Next.js—possibly using dynamic imports and a registry of available hooks.
10. **Internationalization**: port translation files from `ndxzstudio/lang` into Next.js using its built‑in `i18n` support or a library like `next-i18next`.
11. **Caching & performance**: evaluate features like caching flags in `options.php` and replicate them using Next.js middleware or incremental static regeneration.
12. **Testing**: set up unit and integration tests (Jest, React Testing Library) to verify components and API routes.

## 3. Step‑by‑step migration tasks

1. **Initialize** the Next.js project (see `boot-up.md`).
2. **Design document models** for Redis (or your chosen NoSQL store) to hold exhibits, media, sections, and other project data. Write a small data access layer to read and write these JSON blobs.
3. **Implement authentication** and create protected admin routes.
4. **Build admin modules**:
   - Exhibits management (CRUD for pages and media).
   - System settings, sections, and user management.
   - Statistics views if desired.
5. **Convert front-end templates** to React components, starting with the default theme.
6. **Implement dynamic routing** so URLs generated in PHP (for sections, exhibits, tags) resolve via Next.js pages.
7. **Migrate assets** (images, CSS, JS) into the `/public` folder or appropriate static directories.
8. **Develop a plugin framework** in Node that mimics the existing hook system, allowing custom code to inject behavior.
9. **Provide API endpoints** for features currently handled by PHP scripts (such as image processing or AJAX requests found in the PHP modules).
10. **Verify feature parity** with the PHP site by comparing output and behavior for each section.
11. **Deploy** the Next.js application and perform final data migration from the legacy MySQL database into your new Redis/JSON storage.

## 4. Post‑migration cleanup

- Remove unused PHP files once the Next.js version is stable.
- Document how to develop plugins or themes within the new architecture.
- Monitor performance and iterate on caching or optimization strategies.

Following this plan should result in a fully TypeScript-based Next.js application that replicates the functionality of the original Indexhibit PHP site while benefiting from modern tooling and improved maintainability.
