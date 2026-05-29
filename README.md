# 👶 AI Kid to Adult Prediction — Open-Source AI Age Progression & Growth Simulator SaaS (Free FaceApp / AgePro Alternative)

> **Predict photorealistic how a child will look as an adult with AI age progression.** A production-ready, self-hostable Next.js SaaS boilerplate with target age group selection, webhook-backed async delivery, and built-in Stripe billing. A free open-source alternative to FaceApp, AgePro, and Aging Booth — powered by the MuAPI AI engine.

**Tech stack:** Next.js 14 (App Router) · Prisma · PostgreSQL · NextAuth (Google OAuth) · Stripe · Tailwind CSS · MuAPI nano-banana-2-edit · Webhook-backed async delivery
**Use cases:** Family portrait apps · Parenting communities · Social media viral content · Photo gifting · Age prediction tools · Baby photo apps · Creative keepsakes · Family memory apps

![FutureSelf AI Interface](https://cdn.muapi.ai/data/2/294827515637/Screenshot_2026-05-29_165642.png)

## 🌐 Project Details

**GitHub Repository:** [github.com/SamurAIGPT/ai-kid-to-adult-prediction](https://github.com/SamurAIGPT/ai-kid-to-adult-prediction)

**Live Demo:** [ai-kid-to-adult-prediction.vercel.app](https://ai-kid-to-adult-prediction.vercel.app/)

---

FutureSelf AI is a premium SaaS web application that predicts a child's future adult appearance using advanced deep learning. Users upload a photo, select standard vs pro models, configure target age ranges, and interactively compare before/after images using a drag slider.

## ✨ Core Features

### 👶 AI Prediction Studio (Main Page `/`)
- Upload child's portrait selfie via drag-and-drop or file selector
- Fully interactive **guest preview mode** allowing unauthenticated users to explore settings, presets, and sliders, immediately prompting OAuth sign-in only when action triggers are clicked.
- **Dual AI Models**:
  - **Standard (nano-banana-2-edit)**: Fast generation with Google concept search tuning.
  - **Pro (nano-banana-pro-edit)**: High-fidelity enhanced predictions with detailed facial structure modeling.
- **4 Target Age Preset Groups** with pre-filled prompts:
  - 🧑 **Young Adult (20-25 Years)** — Fresh facial details and modern casual look.
  - 💼 **Career Professional (30-35 Years)** — Sophisticated styling with corporate attire.
  - 👑 **Distinguished Adult (40-45 Years)** — Mature features and gentle character lines.
  - 👵 **Mature Elder (50-60 Years)** — Honorable senior style with silver hair details.
- **Dynamic Variable Pricing based on Model and Resolution**:
  - **Standard Model (v2 Edit)**:
    - **1K Resolution**: **12 credits**
    - **2K Resolution**: **18 credits**
    - **4K Resolution**: **24 credits**
  - **Pro Model (Enhanced)**:
    - **1K & 2K Resolution**: **24 credits**
    - **4K Resolution**: **36 credits**
- Draggable Before/After vertical split comparison slider to reveal child-to-adult transformations.

### 🖼️ Personal Predictions Gallery (`/gallery`)
- Responsive CSS grid of completed adult age predictions.
- Detail view modal with full Before/After draggable comparison slider.
- Server-side CORS-bypass download proxy (HD download).
- Auto-refresh gallery every 4 seconds to poll processing generations.

### 💳 Stripe Credit Billing (`/pricing`)
- Four one-time credit packs (no subscriptions):
  - **Basic Pack** ($5 / 1,000 credits — ~83 standard runs)
  - **Standard Pack** ($10 / 2,000 credits — ~166 standard runs)
  - **Professional Pack** ($20 / 4,000 credits — ~333 standard runs — Best Value)
  - **Business Pack** ($50 / 10,000 credits — ~833 standard runs)

### 🔐 Google Auth & live balance syncing
- NextAuth Google Provider with Prisma PostgreSQL adapter.
- Pulse credit balances display in Navbar.

---

## ⚡ Deployment: Vercel & Production

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/SamurAIGPT/ai-kid-to-adult-prediction)

### 🔑 Required Environment Variables

| Service | Variable | Description |
| :--- | :--- | :--- |
| **Database** | `DATABASE_URL` | PostgreSQL connection string (Supabase pooled connection) |
| | `DIRECT_URL` | Direct PostgreSQL connection string |
| **NextAuth** | `NEXTAUTH_SECRET` | Secure random string via `openssl rand -base64 32` |
| | `NEXTAUTH_URL` | Your production domain |
| | `WEBHOOK_URL` | Public URL for MuAPI async callbacks |
| **Google OAuth** | `GOOGLE_CLIENT_ID` | Google Cloud Console OAuth |
| | `GOOGLE_CLIENT_SECRET` | Google Cloud Console OAuth |
| **Stripe** | `STRIPE_SECRET_KEY` | Stripe Secret Key |
| | `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | Stripe Publishable Key |
| | `STRIPE_WEBHOOK_SECRET` | Webhook signing secret |
| **AI** | `MUAPIAPP_API_KEY` | Get from [muapi.ai](https://muapi.ai) |

### 🚀 Production Deployment Setup

1. **Database**: Spin up a PostgreSQL instance.
2. **Import**: Import the forked repo into Vercel.
3. **Environment**: Add all required env keys listed above.
4. **Build Script**: Project builds automatically using `prisma generate && next build`.
5. **Database sync**: Run `npx prisma db push` to generate tables.
6. **Callbacks**:
   - Google: `https://ai-kid-to-adult-prediction.vercel.app/api/auth/callback/google`
   - Stripe Webhook: `https://ai-kid-to-adult-prediction.vercel.app/api/stripe/webhook`
   - MuAPI: `https://ai-kid-to-adult-prediction.vercel.app/api/webhook/muapi`

---

## 🛠️ Local Development

### Prerequisites
- Node.js v18+
- PostgreSQL connection string

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/SamurAIGPT/ai-kid-to-adult-prediction
cd ai-kid-to-adult-prediction

# 2. Install dependencies
npm install

# 3. Setup local environment
cp .env.example .env
# Fill in credentials

# 4. Generate Client & Sync DB
npx prisma generate
npx prisma db push

# 5. Start dev server
npm run dev
```

---

## ⚠️ Database Safety Warning (Shared Pool)

The database is shared across multiple applications. Running `npx prisma db push` on a clean schema will drop other apps' tables. Always follow the **Pull-Declare-Push-Cleanup** sequence:

1. `npx prisma db pull` — Introspect all existing tables into `schema.prisma`
2. Add your `KidAdultPrediction` model and its `User` relation
3. `npx prisma db push` — Safely add new tables and relations
4. Clean `schema.prisma` to keep only `Account`, `Session`, `User`, `VerificationToken`, `KidAdultPrediction`
5. `npx prisma generate` — Rebuild the type-safe Prisma client

---

## 🏗️ Technical Architecture

```
ai-kid-to-adult-prediction/
├── prisma.config.ts          # Dynamic datasource for Prisma v7
├── prisma/
│   └── schema.prisma         # KidAdultPrediction model + NextAuth tables
├── src/
│   ├── app/
│   │   ├── page.js           # Studio Page (upload, age dropdown, comparison slider)
│   │   ├── gallery/page.js   # Personal predictions gallery
│   │   ├── pricing/page.js   # Stripe pricing plans
│   │   └── api/
│   │       ├── auth/         # NextAuth route handler
│   │       ├── upload/       # CDN upload proxy
│   │       ├── generation/   # Credit deduction & variable resolution trigger
│   │       ├── creations/    # GET/DELETE creations with self-healing polling
│   │       ├── download/     # CORS-bypass download proxy
│   │       ├── webhook/muapi/ # MuAPI async callback webhook
│   │       └── stripe/       # Stripe checkout session + webhook
│   ├── components/
│   │   ├── Providers.jsx     # Auth session provider wrapper
│   │   └── layout/Navbar.jsx # Sticky navigation and control headers
│   └── lib/
│       ├── auth.js           # Auth config
│       ├── config.js         # Resolution variable costs (12, 24, 36) and plans
│       ├── prisma.js         # Singleton Prisma client connection pool
│       ├── stripe.js         # Stripe configuration
│       └── services/
│           ├── user.js       # Credits deduction service
│           └── billing.js    # stripe session helper
└── next.config.mjs           # Next image routing config
```

---

## 📄 License

MIT Licensed.

---

_FutureSelf AI: A premium, gold-themed AI growth prediction SaaS built with the Inter font family and Nano Banana._
