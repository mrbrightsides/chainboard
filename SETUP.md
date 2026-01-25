# ChainBoard Setup Guide

Complete installation and configuration guide for **ChainBoard** - a trust-centric Web3 project governance platform.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Detailed Installation](#detailed-installation)
- [Environment Configuration](#environment-configuration)
- [Blockchain Setup](#blockchain-setup)
- [Third-Party Services](#third-party-services)
- [Development](#development)
- [Production Deployment](#production-deployment)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required Software

1. **Node.js** (v18.17 or later)
   - Download: https://nodejs.org/
   - Verify: `node --version`

2. **Package Manager** (choose one)
   - **npm** (comes with Node.js)
   - **pnpm** (recommended): `npm install -g pnpm`
   - **yarn**: `npm install -g yarn`

3. **Git**
   - Download: https://git-scm.com/
   - Verify: `git --version`

### Recommended Tools

- **VS Code** - Code editor
  - Extensions: ESLint, Prettier, Tailwind CSS IntelliSense
- **MetaMask** - Browser wallet for testing
  - Install: https://metamask.io/
- **Postman** - API testing (optional)

### Web3 Requirements

1. **Browser Wallet**
   - MetaMask, WalletConnect, or similar
   - Configured for Ethereum Sepolia testnet

2. **Test ETH** (for NFT minting)
   - Get free Sepolia ETH from faucets:
     - https://sepoliafaucet.com/
     - https://www.infura.io/faucet/sepolia

3. **API Keys** (optional, for full features)
   - OpenAI API key (for AI summaries)
   - Pinata API keys (for IPFS storage)
   - ImgBB API key (for image uploads)

---

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/mrbrightsides/chainboard.git
cd chainboard
```

### 2. Install Dependencies

```bash
# Using npm
npm install

# Or using pnpm (recommended)
pnpm install

# Or using yarn
yarn install
```

### 3. Set Up Environment Variables

```bash
# Copy example environment file
cp .env.example .env.local

# Edit with your preferred editor
nano .env.local
# or
code .env.local
```

### 4. Run Development Server

```bash
# Using npm
npm run dev

# Or using pnpm
pnpm dev

# Or using yarn
yarn dev
```

### 5. Open in Browser

Navigate to **http://localhost:3000**

🎉 **ChainBoard is now running!**

---

## Detailed Installation

### Step 1: Clone Repository

```bash
# Clone via HTTPS
git clone https://github.com/mrbrightsides/chainboard.git

# Or clone via SSH (if configured)
git clone git@github.com:mrbrightsides/chainboard.git

# Navigate to project directory
cd chainboard
```

### Step 2: Install Dependencies

ChainBoard uses several key dependencies:

```json
{
  "dependencies": {
    "next": "^15.3.8",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "ethers": "^6.x",
    "framer-motion": "^11.x",
    "lucide-react": "^0.x",
    "tailwindcss": "^4.x"
  }
}
```

Install all dependencies:

```bash
pnpm install
```

**Installation time:** ~2-5 minutes depending on internet speed.

### Step 3: Verify Installation

```bash
# Check if Next.js is installed correctly
pnpm next --version

# Should output: 15.3.8 or later
```

---

## Environment Configuration

### Environment Variables

Create `.env.local` file in the root directory:

```bash
# ================================
# ChainBoard Environment Variables
# ================================

# -----------------
# Application
# -----------------
NEXT_PUBLIC_APP_NAME=ChainBoard
NEXT_PUBLIC_APP_URL=http://localhost:3000

# -----------------
# Blockchain
# -----------------
# Ethereum Sepolia Testnet
NEXT_PUBLIC_CHAIN_ID=11155111
NEXT_PUBLIC_RPC_URL=https://sepolia.infura.io/v3/YOUR_INFURA_KEY

# NFT Contract (Deploy your own or use default)
NEXT_PUBLIC_NFT_CONTRACT_ADDRESS=0xYourContractAddress

# -----------------
# IPFS Storage (Pinata)
# -----------------
PINATA_API_KEY=your_pinata_api_key
PINATA_SECRET_KEY=your_pinata_secret_key
PINATA_JWT=your_pinata_jwt_token

# -----------------
# AI Services (Optional)
# -----------------
OPENAI_API_KEY=sk-your-openai-api-key

# -----------------
# Image Upload (Optional)
# -----------------
IMGBB_API_KEY=your_imgbb_api_key

# -----------------
# Jitsi Meet (Optional)
# -----------------
NEXT_PUBLIC_JITSI_DOMAIN=meet.jit.si
```

### Required vs Optional Variables

**Required (Minimum Setup):**
- `NEXT_PUBLIC_APP_NAME`
- `NEXT_PUBLIC_APP_URL`

**Optional (Enhanced Features):**
- `NEXT_PUBLIC_CHAIN_ID` - For blockchain features
- `NEXT_PUBLIC_RPC_URL` - For blockchain connectivity
- `PINATA_*` - For IPFS storage (NFT metadata)
- `OPENAI_API_KEY` - For AI meeting summaries
- `IMGBB_API_KEY` - For image uploads

### Security Best Practices

1. **Never commit `.env.local`** - Already in `.gitignore`
2. **Use environment-specific files**
   - `.env.local` - Local development
   - `.env.production` - Production (Vercel auto-detects)
3. **Rotate API keys regularly**
4. **Use restricted API keys** (limit permissions)

---

## Blockchain Setup

### Step 1: Install MetaMask

1. Visit https://metamask.io/
2. Install browser extension
3. Create a new wallet or import existing
4. **Save your seed phrase securely!**

### Step 2: Add Sepolia Testnet

**Automatic (Recommended):**
- Visit https://chainlist.org/
- Search "Sepolia"
- Click "Add to MetaMask"

**Manual:**
1. Open MetaMask
2. Click network dropdown → "Add Network"
3. Enter details:
   ```
   Network Name: Sepolia Testnet
   RPC URL: https://sepolia.infura.io/v3/YOUR_INFURA_KEY
   Chain ID: 11155111
   Currency Symbol: ETH
   Block Explorer: https://sepolia.etherscan.io
   ```

### Step 3: Get Test ETH

Visit a Sepolia faucet:
1. **Infura Faucet**: https://www.infura.io/faucet/sepolia
2. **Alchemy Faucet**: https://sepoliafaucet.com/
3. **PoW Faucet**: https://sepolia-faucet.pk910.de/

**Note:** You'll need ~0.05 SepoliaETH for testing NFT minting.

### Step 4: Configure RPC Provider

**Option A: Infura (Recommended)**
1. Visit https://infura.io/
2. Create free account
3. Create new project
4. Copy project ID
5. Add to `.env.local`:
   ```
   NEXT_PUBLIC_RPC_URL=https://sepolia.infura.io/v3/YOUR_PROJECT_ID
   ```

**Option B: Alchemy**
1. Visit https://www.alchemy.com/
2. Create free account
3. Create app (Ethereum → Sepolia)
4. Copy HTTPS URL
5. Add to `.env.local`

**Option C: Public RPC (Not recommended for production)**
```
NEXT_PUBLIC_RPC_URL=https://rpc.sepolia.org
```

---

## Third-Party Services

See [THIRD_PARTY_APIs.md](./THIRD_PARTY_APIs.md) for detailed API documentation.

### Quick Setup Guide

#### 1. Pinata (IPFS Storage)

**Purpose:** Store NFT metadata on IPFS

1. Visit https://pinata.cloud/
2. Sign up for free account
3. Go to "API Keys" → "New Key"
4. Enable permissions: `pinFileToIPFS`
5. Copy keys to `.env.local`:
   ```
   PINATA_API_KEY=your_api_key
   PINATA_SECRET_KEY=your_secret_key
   PINATA_JWT=your_jwt_token
   ```

#### 2. OpenAI (AI Summaries)

**Purpose:** Generate meeting summaries and task insights

1. Visit https://platform.openai.com/
2. Create account and add payment method
3. Go to "API Keys" → "Create new secret key"
4. Copy to `.env.local`:
   ```
   OPENAI_API_KEY=sk-your-key-here
   ```

**Cost:** ~$0.002 per summary (GPT-4o-mini)

#### 3. ImgBB (Image Upload)

**Purpose:** Host user avatars and project images

1. Visit https://imgbb.com/
2. Sign up for free account
3. Go to "About" → "API"
4. Copy API key to `.env.local`:
   ```
   IMGBB_API_KEY=your_api_key
   ```

**Limits:** 100 uploads/hour (free tier)

---

## Development

### Available Scripts

```bash
# Development server with hot reload
pnpm dev

# Type checking
pnpm type-check

# Build for production
pnpm build

# Start production server
pnpm start

# Lint code
pnpm lint
```

### Development Workflow

1. **Start dev server**: `pnpm dev`
2. **Make changes** to code
3. **Hot reload** automatically updates browser
4. **Check types**: `pnpm type-check`
5. **Test build**: `pnpm build`

### Project Structure

```
chainboard/
├── src/
│   ├── app/                 # Next.js App Router
│   │   ├── layout.tsx      # Root layout
│   │   ├── page.tsx        # Home page
│   │   └── api/            # API routes
│   ├── components/          # React components
│   │   ├── ui/             # UI primitives
│   │   └── ...             # Feature components
│   ├── lib/                # Utilities
│   ├── types/              # TypeScript types
│   └── contexts/           # React contexts
├── public/                  # Static assets
├── .env.local              # Environment variables
├── next.config.js          # Next.js configuration
├── tailwind.config.ts      # Tailwind configuration
├── tsconfig.json           # TypeScript configuration
└── package.json            # Dependencies
```

### Local Testing

#### Test Wallet Connection

1. Open ChainBoard: http://localhost:3000
2. Click "Connect Wallet"
3. Select MetaMask
4. Approve connection
5. Verify wallet address shows in top bar

#### Test NFT Minting

1. Create a task
2. Drag to "Done" column
3. NFT minting should trigger
4. Check transaction in MetaMask
5. Verify on Etherscan after confirmation

#### Test AI Summary (if configured)

1. Create a meeting
2. Add agenda items
3. Click "Generate AI Summary"
4. Check OpenAI API response

---

## Production Deployment

### Deploy to Vercel (Recommended)

**One-Click Deploy:**

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/mrbrightsides/chainboard)

**Manual Deploy:**

1. Install Vercel CLI:
   ```bash
   npm install -g vercel
   ```

2. Login to Vercel:
   ```bash
   vercel login
   ```

3. Deploy:
   ```bash
   vercel
   ```

4. Add environment variables in Vercel dashboard:
   - Project Settings → Environment Variables
   - Add all variables from `.env.local`

5. Redeploy:
   ```bash
   vercel --prod
   ```

### Environment Variables in Vercel

1. Go to **Project Settings** → **Environment Variables**
2. Add each variable:
   - Name: `NEXT_PUBLIC_APP_URL`
   - Value: `https://your-domain.vercel.app`
   - Environment: Production, Preview, Development
3. Click **Save**
4. **Redeploy** for changes to take effect

### Custom Domain

1. Go to **Project Settings** → **Domains**
2. Add custom domain (e.g., `chainboard.elpeef.com`)
3. Update DNS records as instructed
4. Update `.env` with new domain:
   ```
   NEXT_PUBLIC_APP_URL=https://chainboard.elpeef.com
   ```

---

## Troubleshooting

### Common Issues

#### 1. "Module not found" Error

**Problem:** Dependencies not installed

**Solution:**
```bash
# Delete node_modules and lockfile
rm -rf node_modules pnpm-lock.yaml

# Reinstall
pnpm install
```

#### 2. "Port 3000 already in use"

**Problem:** Another app is using port 3000

**Solution:**
```bash
# Kill process on port 3000 (macOS/Linux)
lsof -ti:3000 | xargs kill -9

# Or use different port
PORT=3001 pnpm dev
```

#### 3. Wallet Connection Fails

**Problem:** MetaMask not detected or wrong network

**Solution:**
- Install MetaMask browser extension
- Switch to Sepolia testnet
- Refresh the page
- Hard refresh: Cmd+Shift+R (macOS) or Ctrl+Shift+R (Windows)

#### 4. NFT Minting Fails

**Problem:** Insufficient gas, wrong network, or contract error

**Solution:**
- Check you have Sepolia ETH (get from faucet)
- Verify you're on Sepolia network
- Check contract address in `.env.local`
- Check browser console for error details

#### 5. "Build failed" Error

**Problem:** TypeScript errors or build configuration issue

**Solution:**
```bash
# Check for type errors
pnpm type-check

# Clear Next.js cache
rm -rf .next

# Rebuild
pnpm build
```

#### 6. API Route Errors

**Problem:** Missing environment variables or API limits

**Solution:**
- Verify all required API keys in `.env.local`
- Check API key permissions
- Check API rate limits
- Review server logs in terminal

### Getting Help

If you're still stuck:

1. **Check documentation**
   - [README.md](./README.md)
   - [ARCHITECTURE.md](./ARCHITECTURE.md)
   - [THIRD_PARTY_APIs.md](./THIRD_PARTY_APIs.md)

2. **Search existing issues**
   - https://github.com/mrbrightsides/chainboard/issues

3. **Ask the community**
   - Discord: https://discord.com/channels/@khudri_61362
   - Telegram: https://t.me/khudriakhmad

4. **Contact support**
   - Email: support@elpeef.com

---

## Next Steps

After successful setup:

1. ✅ **Customize branding** - Update logo, colors, name
2. ✅ **Create your first project** - Test task management
3. ✅ **Connect wallet** - Test Web3 features
4. ✅ **Mint an NFT** - Complete a task, verify on-chain
5. ✅ **Explore features** - Meetings, analytics, blockchain panel
6. ✅ **Read documentation** - Understand architecture and APIs
7. ✅ **Join community** - Get help and share feedback

---

## Additional Resources

- **Live Demo**: https://chainboard.elpeef.com
- **GitHub**: https://github.com/mrbrightsides/chainboard
- **Documentation**: See all `.md` files in repository
- **Support**: support@elpeef.com

---

**Happy building! Welcome to the future of trust-centric project governance.** 🚀🛡️

---

**Last Updated**: January 2026  
**Version**: 1.0.0  
**Maintainer**: @mrbrightsides
