# SHAHEEN-OS Setup Guide: Medusa Admin & Next.js Storefront

This guide walks you through setting up the **Medusa Admin Dashboard** (Backend) and the **Next.js Storefront** (Frontend) for your SHAHEEN-OS e-commerce platform.

---

## Part 1: Setting Up Medusa Admin Dashboard

Medusa v2 provides a built-in admin dashboard. Ensure your backend is configured with the necessary dependencies.

### Step 1: Install Admin Dependencies
In your backend project directory (`SHAHEEN-OS`), ensure `@medusajs/medusa` and administrative modules are updated:

```bash
npm install @medusajs/medusa @medusajs/admin
```

### Step 2: Configure `medusa-config.js` or `medusa-config.ts`
Ensure the admin plugin is enabled in your configuration file:

```javascript
module.exports = {
  projectConfig: {
    database_url: process.env.DATABASE_URL,
    type: "postgres",
    store_cors: process.env.STORE_CORS || "http://localhost:8000",
    admin_cors: process.env.ADMIN_CORS || "http://localhost:7000,http://localhost:9000",
  },
  plugins: [
    // ...other plugins
  ],
  modules: {},
};
```

### Step 3: Create an Admin User
Run the Medusa CLI command to generate an administrative account for logging into the dashboard:

```bash
npx medusa user --email admin@shaheen-os.com --password YourSecurePassword123!
```

### Step 4: Run Migrations and Start the Server

```bash
# Run migrations
npx medusa migrations run

# Build project
npm run build

# Start server
npm run start
```

*The admin dashboard will be accessible at:* `http://localhost:9000/app` (or your domain URL on DOM Cloud/Serv00).

---

## Part 2: Setting Up Next.js Storefront

The storefront provides the customer-facing e-commerce store connected to your Medusa backend via REST APIs.

### Step 1: Clone Medusa Next.js Starter
In a separate directory or repository for your frontend, clone the official Medusa Next.js starter storefront:

```bash
npx create-next-app -e https://github.com/medusajs/nextjs-starter-medusa shaheen-storefront
cd shaheen-storefront
```

### Step 2: Configure Environment Variables
Create a `.env.local` file in the root of `shaheen-storefront`:

```env
# URL pointing to your deployed Medusa Backend (SHAHEEN-OS)
NEXT_PUBLIC_MEDUSA_BACKEND_URL=http://localhost:9000

# Publishable API Key generated from Medusa Admin Dashboard
NEXT_PUBLIC_MEDUSA_PUBLISHABLE_KEY=pk_xxxxxxxxxxxxxxxxxxxxxxxxxxxx

# Base URL for the Storefront
NEXT_PUBLIC_BASE_URL=http://localhost:8000
```

### Step 3: Install Dependencies & Run Storefront

```bash
# Install dependencies
npm install

# Run development server
npm run dev
```

*The storefront will be available at:* `http://localhost:8000`

---

## Deployment Summary

1. **Backend (SHAHEEN-OS):** Deploy to **DOM Cloud** or **Serv00** with PostgreSQL connected.
2. **Admin Panel:** Built automatically with the backend and accessible via `/app` endpoint.
3. **Storefront (SHAHEEN Storefront):** Deploy to **Vercel**, **Netlify**, or **DOM Cloud** pointing `NEXT_PUBLIC_MEDUSA_BACKEND_URL` to your live backend domain.