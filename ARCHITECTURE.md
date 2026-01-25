# ChainBoard Architecture

> A comprehensive guide to ChainBoard's system architecture, design decisions, and technical implementation.

---

## Table of Contents

- [Overview](#overview)
- [System Architecture](#system-architecture)
- [Component Architecture](#component-architecture)
- [Data Flow](#data-flow)
- [Blockchain Integration](#blockchain-integration)
- [Storage Strategy](#storage-strategy)
- [Security Architecture](#security-architecture)
- [Performance Optimization](#performance-optimization)
- [Scalability Considerations](#scalability-considerations)

---

## Overview

ChainBoard is built as a **trust-centric Web3 project governance platform** that combines traditional project management capabilities with blockchain-powered accountability and verification systems.

### Core Architectural Principles

1. **Transparency First** - All critical actions are traceable and verifiable
2. **Decentralized Trust** - No single point of failure for trust verification
3. **Progressive Enhancement** - Works offline, enhanced with Web3
4. **Mobile-First Responsive** - Touch-optimized, desktop-enhanced
5. **Type-Safe Everything** - Strict TypeScript throughout

---

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         ChainBoard                          │
│                     (Next.js 15 App)                        │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Frontend   │     │   Backend    │     │  Blockchain  │
│              │     │              │     │              │
│ • React UI   │────▶│ • API Routes │────▶│ • Base L2    │
│ • State Mgmt │     │ • Proxy      │     │ • NFT Mint   │
│ • LocalStore │     │ • AI Service │     │ • IPFS       │
└──────────────┘     └──────────────┘     └──────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
                    ┌──────────────────┐
                    │  External APIs   │
                    │                  │
                    │ • OpenAI (AI)    │
                    │ • Jitsi (Video)  │
                    │ • ImgBB (Upload) │
                    └──────────────────┘
```

### Technology Stack Layers

#### 1. Presentation Layer
- **Framework**: Next.js 15.3.8+ (App Router)
- **UI Library**: React 18+
- **Styling**: Tailwind CSS v4
- **Components**: shadcn/ui (Radix UI primitives)
- **Icons**: Lucide React
- **Animations**: Framer Motion

#### 2. Application Layer
- **Language**: TypeScript 5+ (Strict mode)
- **State Management**: React Context + Hooks
- **Local Storage**: Browser LocalStorage API
- **Form Handling**: React Hook Form + Zod validation

#### 3. API Layer
- **Backend**: Next.js API Routes (Server-side)
- **Proxy Service**: Custom API proxy (`/api/proxy`)
- **AI Integration**: OpenAI GPT-4 API
- **Video Conference**: Jitsi Meet SDK

#### 4. Blockchain Layer
- **Network**: Ethereum Sepolia Testnet
- **Web3 Library**: Ethers.js v6
- **Authentication**: SIWE (Sign-In with Ethereum)
- **Smart Contracts**: ERC-721 (NFT standard)
- **Storage**: IPFS (via Pinata)

---

## Component Architecture

### Directory Structure

```
src/
├── app/                          # Next.js App Router
│   ├── layout.tsx               # Root layout with providers
│   ├── page.tsx                 # Main dashboard entry
│   └── api/                     # API routes
│       ├── proxy/               # External API proxy
│       ├── openai/              # AI services
│       └── jitsi/               # Video conference
│
├── components/                   # React components
│   ├── ui/                      # shadcn/ui primitives
│   │   ├── button.tsx
│   │   ├── dialog.tsx
│   │   ├── input.tsx
│   │   └── ...
│   │
│   ├── EnhancedTopBar.tsx       # Navigation & auth
│   ├── ProjectSidebar.tsx       # Project navigation
│   ├── KanbanBoard.tsx          # Task management
│   ├── DashboardPanel.tsx       # Trust metrics
│   ├── BlockchainPanel.tsx      # Proof of contribution
│   ├── MeetingPanel.tsx         # Governance meetings
│   ├── AnalyticsPanel.tsx       # Insights & metrics
│   └── AboutPanel.tsx           # Platform info
│
├── contexts/                     # React Context providers
│   ├── AppContext.tsx           # Global app state
│   └── Web3Context.tsx          # Blockchain state
│
├── lib/                         # Utilities & helpers
│   ├── utils.ts                 # Common utilities
│   ├── blockchain.ts            # Web3 helpers
│   └── storage.ts               # LocalStorage wrapper
│
└── types/                       # TypeScript definitions
    ├── index.ts                 # Core types
    └── blockchain.ts            # Web3 types
```

### Component Hierarchy

```
App (layout.tsx)
├── AppProvider (Global State)
│   └── Web3Provider (Blockchain State)
│       └── Page (page.tsx)
│           ├── EnhancedTopBar
│           │   ├── Navigation
│           │   ├── WalletConnect
│           │   ├── ThemeToggle
│           │   ├── Notifications
│           │   └── Settings
│           │
│           ├── ProjectSidebar
│           │   └── ProjectList
│           │
│           └── MainContent (Dynamic)
│               ├── DashboardPanel
│               │   ├── TrustMetrics
│               │   ├── StatsCards
│               │   └── ActivityChart
│               │
│               ├── KanbanBoard
│               │   ├── TaskColumn (To Do)
│               │   ├── TaskColumn (In Progress)
│               │   ├── TaskColumn (Done)
│               │   └── TaskCard
│               │       ├── TaskDetails
│               │       ├── Comments
│               │       └── Attachments
│               │
│               ├── BlockchainPanel
│               │   ├── TrustScore
│               │   ├── ContributionRecords
│               │   └── OnChainActivity
│               │
│               ├── MeetingPanel
│               │   ├── MeetingList
│               │   ├── JitsiIntegration
│               │   └── AISummary
│               │
│               ├── AnalyticsPanel
│               │   ├── PerformanceMetrics
│               │   ├── TaskDistribution
│               │   └── TeamProductivity
│               │
│               └── AboutPanel
│                   ├── ComparisonTable
│                   ├── UseCases
│                   └── Philosophy
```

---

## Data Flow

### 1. User Authentication Flow (SIWE)

```
User → Connect Wallet
  → MetaMask/WalletConnect prompts signature
    → Sign SIWE message
      → Verify signature
        → Store wallet address in state
          → Enable blockchain features
```

### 2. Task Management Flow

```
User creates task
  → Store in LocalStorage (offline-first)
    → Update UI immediately
      → Sync to state management
        → If wallet connected:
          → Prepare NFT metadata
            → Upload to IPFS (Pinata)
              → Mint NFT on-chain
                → Store transaction hash
                  → Update trust metrics
```

### 3. Meeting Management Flow

```
User schedules meeting
  → Store meeting details locally
    → Start Jitsi video conference
      → Conduct meeting
        → End meeting
          → Trigger AI summary
            → Call OpenAI API
              → Generate summary
                → Store as governance record
                  → Update analytics
```

### 4. Blockchain Verification Flow

```
Task completed
  → Generate proof metadata
    → Upload to IPFS
      → Get IPFS hash (CID)
        → Create NFT with metadata URI
          → Sign transaction
            → Broadcast to Ethereum Sepolia
              → Wait for confirmation
                → Store transaction hash
                  → Display in Blockchain Panel
                    → Verifiable on Etherscan
```

---

## Blockchain Integration

### Smart Contract Architecture

ChainBoard uses standard **ERC-721 NFT contracts** for proof of contribution.

#### NFT Metadata Structure

```json
{
  "name": "ChainBoard Contribution #001",
  "description": "Proof of contribution for completing: [Task Title]",
  "image": "ipfs://[IPFS_HASH]",
  "attributes": [
    {
      "trait_type": "Task Title",
      "value": "Review Q1 ESG Impact Report"
    },
    {
      "trait_type": "Priority",
      "value": "High"
    },
    {
      "trait_type": "Project",
      "value": "ESG Initiative 2026"
    },
    {
      "trait_type": "Completion Date",
      "value": "2026-01-15T10:30:00Z"
    },
    {
      "trait_type": "Contributor",
      "value": "0x742d35Cc6634C0532925a3b844Bc454e4438f44e"
    }
  ]
}
```

#### IPFS Storage Flow

1. **Metadata Generation** - Create JSON with task details
2. **Upload to Pinata** - Store metadata on IPFS
3. **Get CID** - Retrieve content identifier (IPFS hash)
4. **Mint NFT** - Create NFT with `tokenURI = ipfs://[CID]`
5. **Transaction Confirmation** - Wait for blockchain confirmation
6. **Store Record** - Save transaction hash locally

### Web3 Provider Setup

```typescript
// Ethers.js v6 Integration
import { BrowserProvider, Contract } from 'ethers';

// Connect to wallet
const provider = new BrowserProvider(window.ethereum);
const signer = await provider.getSigner();

// NFT Contract interaction
const nftContract = new Contract(
  NFT_CONTRACT_ADDRESS,
  NFT_ABI,
  signer
);

// Mint NFT
const tx = await nftContract.mint(
  recipientAddress,
  tokenURI // ipfs://[IPFS_HASH]
);

await tx.wait(); // Wait for confirmation
```

---

## Storage Strategy

### LocalStorage Schema

ChainBoard uses browser LocalStorage for offline-first functionality:

```typescript
// Storage Keys
localStorage.setItem('chainboard_projects', JSON.stringify(projects));
localStorage.setItem('chainboard_tasks', JSON.stringify(tasks));
localStorage.setItem('chainboard_meetings', JSON.stringify(meetings));
localStorage.setItem('chainboard_user_profile', JSON.stringify(profile));
localStorage.setItem('chainboard_preferences', JSON.stringify(prefs));
localStorage.setItem('chainboard_nft_records', JSON.stringify(nfts));
```

#### Data Models

**Project Model:**
```typescript
interface Project {
  id: string;
  name: string;
  description: string;
  createdAt: string;
  updatedAt: string;
  tasks: string[]; // task IDs
  members: string[]; // wallet addresses
  color: string;
}
```

**Task Model:**
```typescript
interface Task {
  id: string;
  title: string;
  description: string;
  status: 'todo' | 'in-progress' | 'done';
  priority: 'low' | 'medium' | 'high';
  assignee: string; // wallet address
  projectId: string;
  deadline: string;
  createdAt: string;
  completedAt?: string;
  nftMinted?: boolean;
  nftTransactionHash?: string;
  ipfsHash?: string;
}
```

**NFT Record Model:**
```typescript
interface NFTRecord {
  id: string;
  taskId: string;
  tokenId: string;
  transactionHash: string;
  ipfsHash: string;
  contractAddress: string;
  mintedAt: string;
  contributor: string; // wallet address
  metadata: {
    name: string;
    description: string;
    image: string;
    attributes: Array<{
      trait_type: string;
      value: string;
    }>;
  };
}
```

### Data Persistence Strategy

1. **Write-Through Cache** - All updates go to LocalStorage immediately
2. **Optimistic UI** - UI updates before blockchain confirmation
3. **Background Sync** - Blockchain operations happen asynchronously
4. **Conflict Resolution** - Last-write-wins for local edits
5. **Export/Import** - Users can export data as JSON backup

---

## Security Architecture

### Authentication & Authorization

1. **SIWE (Sign-In with Ethereum)**
   - No passwords, wallet-based identity
   - Cryptographic signature verification
   - Message format: EIP-4361 compliant

2. **API Security**
   - API routes protected with CORS
   - Proxy route validates request origin
   - No sensitive keys exposed client-side

3. **Smart Contract Security**
   - Standard ERC-721 implementation
   - No custom logic (reduces attack surface)
   - Testnet deployment for safety

### Data Privacy

1. **Local-First Storage** - User data stays in browser
2. **No Backend Database** - No centralized data collection
3. **IPFS Metadata** - Public but pseudonymous (wallet addresses)
4. **Optional Blockchain** - Users can use app without Web3

---

## Performance Optimization

### 1. Code Splitting
- Next.js automatic code splitting
- Dynamic imports for heavy components
- Route-based chunking

### 2. Image Optimization
- Next.js Image component with lazy loading
- WebP format support
- Responsive image sizing

### 3. Caching Strategy
- LocalStorage for persistent cache
- React Context for in-memory state
- Browser cache for static assets

### 4. Rendering Strategy
- Server components where possible
- Client components only when needed
- Streaming SSR for faster TTFB

### 5. Bundle Size
- Tree-shaking unused code
- Minification in production
- Gzip compression

---

## Scalability Considerations

### Current Architecture Limits

1. **LocalStorage Limit** - ~5-10MB per domain
   - Mitigation: Data export/import, pagination
   
2. **Blockchain Costs** - Gas fees for NFT minting
   - Mitigation: Batch minting, L2 solutions (Base)

3. **API Rate Limits** - External services (OpenAI, IPFS)
   - Mitigation: Queuing, retry logic, fallbacks

### Future Scaling Paths

1. **Phase 2: Enhanced Governance**
   - Decentralized storage (Arweave, Filecoin)
   - On-chain governance records
   - Multi-chain support

2. **Phase 3: Ecosystem Expansion**
   - Backend database for team collaboration
   - WebSocket for real-time sync
   - CDN for global performance

---

## Design Decisions

### Why Next.js 15?
- App Router for modern routing
- Server Components for performance
- Built-in API routes for backend logic
- Vercel deployment optimization

### Why LocalStorage over Database?
- **Privacy**: User data stays local
- **Offline**: Works without internet
- **Cost**: No backend infrastructure
- **Speed**: Instant read/write
- **Simplicity**: No auth/sync complexity

### Why Ethereum Sepolia?
- **Testnet**: No real ETH needed
- **Compatibility**: Standard EVM chain
- **Tools**: Etherscan, MetaMask support
- **Future**: Easy mainnet migration

### Why IPFS for Metadata?
- **Decentralization**: No single point of failure
- **Permanence**: Content-addressed storage
- **Verifiability**: CID = cryptographic hash
- **Standard**: NFT industry best practice

---

## Monitoring & Debugging

### Error Handling

1. **Client-Side Errors**
   - Try-catch blocks for async operations
   - Toast notifications for user feedback
   - Console logging in development

2. **Blockchain Errors**
   - Transaction failure handling
   - Gas estimation errors
   - Network connectivity issues

3. **API Errors**
   - Retry logic with exponential backoff
   - Fallback to cached data
   - User-friendly error messages

### Performance Monitoring

- Browser DevTools for client-side metrics
- Next.js build analyzer for bundle size
- Lighthouse for Core Web Vitals

---

## Contributing to Architecture

For proposing architectural changes:

1. Open an issue with `[ARCHITECTURE]` prefix
2. Describe the problem and proposed solution
3. Consider backward compatibility
4. Include migration path if breaking changes

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

---

## Additional Resources

- **Live Demo**: https://chainboard.elpeef.com
- **GitHub**: https://github.com/mrbrightsides/chainboard
- **Contact**: support@elpeef.com

---

**Last Updated**: January 2026  
**Version**: 1.0.0  
**Maintainer**: @mrbrightsides
