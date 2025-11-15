# Production Deployment Checklist

## Security

- [ ] Set `NEXTAUTH_SECRET` to a strong random string
- [ ] Configure production `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET`
- [ ] Set `NEXTAUTH_URL` to production domain
- [ ] Enable HTTPS
- [ ] Configure database backup

## Environment Variables

- [ ] Create `.env.production` file
- [ ] Set all required environment variables
- [ ] Ensure `.env` file is not included in Git (check `.gitignore`)

## Database

- [ ] Run `npx prisma migrate deploy`
- [ ] Seed data (if necessary)
- [ ] Configure database file backup

## Build

- [ ] Clean install dependencies with `npm ci`
- [ ] Generate Prisma Client with `npx prisma generate`
- [ ] Build without errors with `npm run build`

## Performance

- [ ] Configure image optimization
- [ ] Configure caching
- [ ] Configure CDN (static assets)

## Monitoring

- [ ] Configure error logging
- [ ] Configure access logging
- [ ] Configure performance monitoring

## Startup

```bash
# Normal startup
npm start

# Using PM2 (recommended)
pm2 start npm --name "monst_web" -- start
pm2 startup
pm2 save

```

## Environment Variables Template

```env
# Database
DATABASE_URL="file:./prod.db"

# NextAuth
NEXTAUTH_URL="https://your-domain.com"
NEXTAUTH_SECRET="generate-with-openssl-rand-base64-32"

# Google OAuth
GOOGLE_CLIENT_ID="your-production-client-id.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="your-production-client-secret"
```

## Troubleshooting

### Build Errors
```bash
rm -rf .next node_modules
npm ci
npx prisma generate
npm run build
```

### Database Errors
```bash
npx prisma migrate deploy
npx prisma generate
```

### Permission Errors
```bash
chmod 755 .
chmod 644 prisma/prod.db
```

## Running Migrations and Seeds with GitHub Actions

Using the workflow `.github/workflows/prisma-migrate-deploy.yml` included in the repository, you can run migrations and seeds in CI when pushing to the `main` branch.

Steps:
1. Copy the "Direct DB connection (port 5432)" connection string from Supabase.
2. Paste it as `PROD_DATABASE_URL` in GitHub repository Settings → Secrets → Actions.
3. When pushing to `main`, the workflow starts and runs `npx prisma migrate deploy` and `npx prisma db seed`.

Notes:
- Use Direct connection (5432) in CI. Pooler (pgBouncer) endpoints can cause issues with DDL operations.
- You can use connection pooler in Vercel runtime environment, but it's safer to use Direct connection in CI for migrations.

## Configuring pgBouncer for Vercel

When using Supabase's pgBouncer (or connection pooler), **add the `?pgbouncer=true` parameter to environment variables** to prevent Prisma prepared statement conflicts.

Steps:
1. Go to Vercel Project Settings → Environment Variables
2. Edit `DATABASE_URL` and add `?pgbouncer=true` at the end
   ```
   Example: postgresql://postgres:PASSWORD@db.xxx.supabase.co:5432/postgres?pgbouncer=true
   ```
3. Apply to all environments: Production, Preview, Development
4. Redeploy

This makes Prisma disable prepared statements and operate in compatibility mode with pgBouncer. This resolves the error "prepared statement \"sX\" already exists (42P05)".

