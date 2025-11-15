# MonsterStrike Character Manager

A web application for managing Monster Strike character collections and Wakuwaku no Mi (abilities) across multiple game accounts.

[![Next.js](https://img.shields.io/badge/Next.js-15.0-black?logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue?logo=typescript)](https://www.typescriptlang.org/)
[![Prisma](https://img.shields.io/badge/Prisma-6.0-2D3748?logo=prisma)](https://www.prisma.io/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

## ✨ Key Features

### Account Management
- 🎮 **Multiple Accounts**: Manage up to 4 game accounts per user
- 👥 **Friend System**: Connect with other users and share collection status
- 🔍 **Cross-Account Search**: Check character ownership across all accounts at once

### Character Management
- 📊 **Detailed Collection Tracking**: Support for multiple copies of the same character (including pre/post evolution)
- 🎯 **Wakuwaku no Mi**: Attach up to 3 abilities per character with bulk editing and preset support
- 🔎 **Advanced Search**: Filter by element and type, with OR search functionality

### UI/UX
- 🌙 **Dark Mode**: Toggle between light and dark themes
- 📱 **Responsive Design**: Optimized for mobile, tablet, and desktop
- ⚡ **Performance**: Scroll position preservation and optimized builds

### Authentication
- 🔐 **Google OAuth**: Sign in with your Google account
- 🔒 **Secure**: Authentication managed by NextAuth.js

## 🛠️ Tech Stack

- **Framework**: [Next.js 15](https://nextjs.org/) (App Router)
- **Language**: [TypeScript 5](https://www.typescriptlang.org/)
- **Database**: [PostgreSQL](https://www.postgresql.org/) (Prisma ORM)
- **Authentication**: [NextAuth.js 4](https://next-auth.js.org/)
- **Styling**: [Tailwind CSS 3](https://tailwindcss.com/)
- **Deployment**: [Vercel](https://vercel.com/)

## 🚀 Setup

### Prerequisites

- Node.js 18 or higher
- PostgreSQL database (free plans available at [Supabase](https://supabase.com/) or [Neon](https://neon.tech/))
- Google account (for OAuth authentication)

### 1. Clone the Repository

```bash
git clone https://github.com/tamura09/monst_web.git
cd monst_web
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Set the following environment variables:

```env
# Database connection URL
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE"

# NextAuth.js configuration
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-secret-key-here"  # Generate with: openssl rand -base64 32

# Google OAuth credentials (obtain from https://console.cloud.google.com/)
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"
```

### 4. Setup Database

```bash
# Run Prisma migrations
npx prisma migrate deploy

# Generate Prisma client
npx prisma generate
```

### 5. Start Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## 📖 Google OAuth Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Navigate to "APIs & Services" → "Credentials"
4. Click "Create Credentials" → "OAuth client ID"
5. Application type: **Web application**
6. Add authorized redirect URIs:
   - `http://localhost:3000/api/auth/callback/google` (development)
   - `https://yourdomain.com/api/auth/callback/google` (production)
7. Copy the client ID and secret to your `.env` file

## 💻 Development

### Database Management

Manage your data visually with Prisma Studio:

```bash
npx prisma studio
```

Open [http://localhost:5555](http://localhost:5555) to view and edit database contents.

### Build

```bash
npm run build
```

### Production

```bash
npm start
```

## 📁 Project Structure

```
monst_web/
├── app/                      # Next.js App Router
│   ├── page.tsx             # Home (character search)
│   ├── layout.tsx           # Layout
│   ├── accounts/            # Account management
│   ├── characters/          # Character list
│   ├── profile/             # Profile editing
│   ├── friends/             # Friend management
│   └── api/                 # API Routes
│       ├── auth/            # NextAuth.js authentication
│       ├── accounts/        # Account API
│       ├── characters/      # Character API
│       └── friends/         # Friend API
├── components/               # React components
│   ├── Navigation.tsx       # Navigation
│   ├── ThemeProvider.tsx    # Dark mode management
│   └── ...
├── lib/
│   ├── prisma.ts            # Prisma client
│   └── string-utils.ts      # String utilities
├── prisma/
│   ├── schema.prisma        # Database schema
│   └── migrations/          # Migration history
├── docs/                     # Development documentation
└── package.json
```

## 🗄️ Database Schema

Key models:

- **User**: User information (Google OAuth linked)
- **GameAccount**: Game accounts (up to 4 per user)
- **CharacterMaster**: Character master data
- **OwnedCharacter**: Owned characters (supports multiple copies)
- **WakuwakuMaster**: Wakuwaku no Mi master data
- **OwnedCharacterWakuwaku**: Equipped Wakuwaku no Mi (up to 3 per character)
- **Friend**: Friend relationships
- **FriendRequest**: Friend requests

See [prisma/schema.prisma](prisma/schema.prisma) for details.

## 🚢 Deployment

### Deploy to Vercel

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/tamura09/monst_web)

1. Import your project in Vercel
2. Set environment variables (refer to `.env.example`)
3. Connect a PostgreSQL database (Supabase/Neon, etc.)
4. Deploy

See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for details.

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

## 📮 Contact

If you have questions or suggestions, please create an [Issue](https://github.com/tamura09/monst_web/issues).