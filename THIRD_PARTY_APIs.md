# Third-Party APIs Documentation

Complete reference for all external APIs and services integrated into **ChainBoard**.

---

## Table of Contents

- [Overview](#overview)
- [Blockchain APIs](#blockchain-apis)
- [Storage APIs](#storage-apis)
- [AI Services](#ai-services)
- [Media Services](#media-services)
- [Communication Services](#communication-services)
- [Rate Limits & Quotas](#rate-limits--quotas)
- [Error Handling](#error-handling)
- [Security Considerations](#security-considerations)

---

## Overview

ChainBoard integrates with several third-party services to enable trust-centric governance features:

| Service | Purpose | Required | Free Tier |
|---------|---------|----------|-----------|
| Ethereum Sepolia | Blockchain network | Yes (for Web3) | ✅ Testnet |
| Infura/Alchemy | RPC provider | Recommended | ✅ Yes |
| Pinata | IPFS storage | Yes (for NFTs) | ✅ 1GB free |
| OpenAI | AI summaries | No | ❌ Paid only |
| ImgBB | Image hosting | No | ✅ Free tier |
| Jitsi Meet | Video conferencing | No | ✅ Free |

---

## Blockchain APIs

### Ethereum Sepolia Testnet

**Purpose:** Execute smart contracts, mint NFTs, verify on-chain proofs

**Network Details:**
```
Network Name: Sepolia Testnet
Chain ID: 11155111
RPC URL: https://sepolia.infura.io/v3/YOUR_KEY
Explorer: https://sepolia.etherscan.io
Symbol: ETH
```

**Setup:**

1. Get test ETH from faucets:
   - https://sepoliafaucet.com/
   - https://www.infura.io/faucet/sepolia
   - https://sepolia-faucet.pk910.de/

2. Configure in `.env.local`:
   ```bash
   NEXT_PUBLIC_CHAIN_ID=11155111
   NEXT_PUBLIC_RPC_URL=https://sepolia.infura.io/v3/YOUR_INFURA_KEY
   ```

**Usage in Code:**

```typescript
import { BrowserProvider } from 'ethers';

// Connect to Sepolia via MetaMask
const provider = new BrowserProvider(window.ethereum);

// Request network switch if needed
await provider.send('wallet_switchEthereumChain', [
  { chainId: '0xaa36a7' } // 11155111 in hex
]);

// Get signer
const signer = await provider.getSigner();
```

**Costs:**
- ✅ **Free** - Testnet (no real ETH required)
- Gas fees paid in test ETH

---

### Infura (RPC Provider)

**Purpose:** Connect to Ethereum network without running a node

**Documentation:** https://docs.infura.io/

**Setup:**

1. Create account: https://infura.io/
2. Create new project
3. Copy Project ID
4. Add to `.env.local`:
   ```bash
   NEXT_PUBLIC_RPC_URL=https://sepolia.infura.io/v3/YOUR_PROJECT_ID
   ```

**Free Tier:**
- 100,000 requests/day
- Archive data access
- WebSocket support

**Usage:**

```typescript
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider(
  process.env.NEXT_PUBLIC_RPC_URL
);

// Get block number
const blockNumber = await provider.getBlockNumber();

// Get transaction receipt
const receipt = await provider.getTransactionReceipt(txHash);
```

**Rate Limits:**
- 100,000 requests/day (free)
- 10 requests/second

**Alternative:** Alchemy (https://www.alchemy.com/)

---

### Etherscan API (Optional)

**Purpose:** Verify transactions, fetch contract data

**Documentation:** https://docs.etherscan.io/

**Setup:**

1. Create account: https://etherscan.io/register
2. Generate API key: https://etherscan.io/myapikey
3. Add to `.env.local`:
   ```bash
   ETHERSCAN_API_KEY=your_api_key
   ```

**Usage:**

```typescript
// Get transaction details
const url = `https://api-sepolia.etherscan.io/api?module=transaction&action=gettxinfo&txhash=${txHash}&apikey=${ETHERSCAN_API_KEY}`;

const response = await fetch(url);
const data = await response.json();
```

**Free Tier:**
- 5 requests/second
- 100,000 requests/day

---

## Storage APIs

### Pinata (IPFS)

**Purpose:** Decentralized storage for NFT metadata

**Documentation:** https://docs.pinata.cloud/

**Setup:**

1. Create account: https://pinata.cloud/
2. Go to API Keys → Create New Key
3. Enable `pinFileToIPFS` permission
4. Copy keys to `.env.local`:
   ```bash
   PINATA_API_KEY=your_api_key
   PINATA_SECRET_KEY=your_secret_key
   PINATA_JWT=your_jwt_token
   ```

**Usage:**

```typescript
// Upload JSON metadata to IPFS
async function uploadToIPFS(metadata: object): Promise<string> {
  const formData = new FormData();
  const blob = new Blob([JSON.stringify(metadata)], { type: 'application/json' });
  formData.append('file', blob, 'metadata.json');

  const response = await fetch('https://api.pinata.cloud/pinning/pinFileToIPFS', {
    method: 'POST',
    headers: {
      'pinata_api_key': process.env.PINATA_API_KEY!,
      'pinata_secret_api_key': process.env.PINATA_SECRET_KEY!,
    },
    body: formData,
  });

  const data = await response.json();
  return data.IpfsHash; // Returns CID
}

// Use in NFT minting
const metadata = {
  name: 'ChainBoard Contribution #001',
  description: 'Proof of contribution',
  image: 'ipfs://...',
  attributes: [...]
};

const ipfsHash = await uploadToIPFS(metadata);
const tokenURI = `ipfs://${ipfsHash}`;
```

**Free Tier:**
- 1 GB storage
- Unlimited pins
- 100 requests/second

**Pricing:**
- Free: 1 GB
- Picnic: $20/month (100 GB)
- Submarine: $200/month (1 TB)

**Rate Limits:**
- 180 requests/minute

**Best Practices:**
- Pin important files
- Use descriptive filenames
- Compress large files
- Monitor storage usage

---

## AI Services

### OpenAI API

**Purpose:** Generate meeting summaries, task insights, AI intelligence

**Documentation:** https://platform.openai.com/docs

**Setup:**

1. Create account: https://platform.openai.com/
2. Add payment method
3. Generate API key: https://platform.openai.com/api-keys
4. Add to `.env.local`:
   ```bash
   OPENAI_API_KEY=sk-your-key-here
   ```

**Usage:**

```typescript
// Generate meeting summary
async function generateMeetingSummary(transcript: string): Promise<string> {
  const response = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${process.env.OPENAI_API_KEY}`,
    },
    body: JSON.stringify({
      model: 'gpt-4o-mini',
      messages: [
        {
          role: 'system',
          content: 'You are a professional meeting summarizer. Create concise, actionable summaries.'
        },
        {
          role: 'user',
          content: `Summarize this meeting transcript:\n\n${transcript}`
        }
      ],
      temperature: 0.7,
      max_tokens: 500,
    }),
  });

  const data = await response.json();
  return data.choices[0].message.content;
}
```

**Models Used:**
- `gpt-4o-mini` - Fast, cost-effective ($0.15/1M input tokens)
- `gpt-4o` - Advanced reasoning ($5/1M input tokens)

**Pricing (as of 2026):**
- GPT-4o-mini: ~$0.002 per summary
- GPT-4o: ~$0.015 per summary

**Rate Limits:**
- Tier 1 (free trial): 3 RPM, 200 RPD
- Tier 2 ($5+ spent): 3,500 RPM, 10,000 RPD

**Error Handling:**

```typescript
try {
  const summary = await generateMeetingSummary(transcript);
} catch (error: any) {
  if (error.status === 429) {
    // Rate limit exceeded
    console.error('Too many requests. Please wait.');
  } else if (error.status === 401) {
    // Invalid API key
    console.error('Invalid API key.');
  } else {
    console.error('AI summary failed:', error.message);
  }
}
```

---

## Media Services

### ImgBB

**Purpose:** Host user avatars, project images, attachments

**Documentation:** https://api.imgbb.com/

**Setup:**

1. Create account: https://imgbb.com/
2. Get API key: https://api.imgbb.com/
3. Add to `.env.local`:
   ```bash
   IMGBB_API_KEY=your_api_key
   ```

**Usage:**

```typescript
// Upload image
async function uploadImage(file: File): Promise<string> {
  const formData = new FormData();
  formData.append('image', file);
  formData.append('key', process.env.IMGBB_API_KEY!);

  const response = await fetch('https://api.imgbb.com/1/upload', {
    method: 'POST',
    body: formData,
  });

  const data = await response.json();
  
  if (data.success) {
    return data.data.url; // Direct image URL
  }
  
  throw new Error(data.error.message);
}

// Use in profile avatar upload
const avatarUrl = await uploadImage(file);
```

**Free Tier:**
- Unlimited uploads
- 32 MB max file size
- No bandwidth limits
- 100 uploads/hour

**Supported Formats:**
- JPG, PNG, BMP, GIF, WEBP, HEIC

**Response Format:**

```json
{
  "success": true,
  "data": {
    "url": "https://i.ibb.co/abc123/image.jpg",
    "display_url": "https://i.ibb.co/abc123/image.jpg",
    "delete_url": "https://ibb.co/abc123/delete",
    "size": 12345,
    "time": 1234567890,
    "expiration": 0
  }
}
```

**Rate Limits:**
- 100 uploads/hour (free)
- No daily limit

---

## Communication Services

### Jitsi Meet

**Purpose:** Built-in video conferencing for governance meetings

**Documentation:** https://jitsi.github.io/handbook/

**Setup:**

1. No API key required (free public instance)
2. Configure in `.env.local` (optional):
   ```bash
   NEXT_PUBLIC_JITSI_DOMAIN=meet.jit.si
   ```

**Usage:**

```typescript
import { JitsiMeeting } from '@jitsi/react-sdk';

// Embed Jitsi meeting
<JitsiMeeting
  domain="meet.jit.si"
  roomName={`chainboard-meeting-${meetingId}`}
  configOverwrite={{
    startWithAudioMuted: true,
    startWithVideoMuted: false,
    disableModeratorIndicator: false,
    prejoinPageEnabled: true,
  }}
  interfaceConfigOverwrite={{
    SHOW_JITSI_WATERMARK: false,
    TOOLBAR_BUTTONS: [
      'microphone', 'camera', 'closedcaptions', 'desktop',
      'fullscreen', 'fodeviceselection', 'hangup', 'profile',
      'chat', 'recording', 'livestreaming', 'raisehand',
      'videoquality', 'filmstrip', 'feedback', 'settings',
      'tileview', 'download', 'help', 'mute-everyone'
    ],
  }}
  userInfo={{
    displayName: userName,
    email: userEmail,
  }}
  onApiReady={(externalApi) => {
    // Meeting ready
  }}
  getIFrameRef={(iframeRef) => {
    iframeRef.style.height = '600px';
  }}
/>
```

**Free Tier:**
- ✅ Unlimited meetings
- ✅ No time limits
- ✅ Up to 75 participants
- ✅ Screen sharing
- ✅ Recording
- ✅ Chat

**Self-Hosted Option:**
- Deploy your own Jitsi instance
- Full control and customization
- See: https://jitsi.github.io/handbook/docs/devops-guide/

**Privacy:**
- End-to-end encryption available
- GDPR compliant
- No account required

---

## Rate Limits & Quotas

### Summary Table

| Service | Free Tier Limit | Upgrade Path |
|---------|----------------|--------------|
| Infura | 100k req/day | $50/month (Starter) |
| Pinata | 1 GB storage | $20/month (100 GB) |
| OpenAI | 3 RPM (trial) | $5 spend → 3,500 RPM |
| ImgBB | 100 uploads/hour | No paid tier |
| Jitsi | Unlimited | Self-host for control |
| Etherscan | 100k req/day | No paid tier |

### Handling Rate Limits

**Exponential Backoff:**

```typescript
async function fetchWithRetry(
  url: string,
  options: RequestInit,
  maxRetries = 3
): Promise<Response> {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);
      
      if (response.status === 429) {
        // Rate limited - wait and retry
        const waitTime = Math.pow(2, i) * 1000; // 1s, 2s, 4s
        await new Promise(resolve => setTimeout(resolve, waitTime));
        continue;
      }
      
      return response;
    } catch (error) {
      if (i === maxRetries - 1) throw error;
    }
  }
  
  throw new Error('Max retries exceeded');
}
```

---

## Error Handling

### Common Errors

**1. Authentication Errors (401)**

```typescript
if (response.status === 401) {
  console.error('Invalid API key. Check environment variables.');
  // Fallback to cached data or disable feature
}
```

**2. Rate Limit Errors (429)**

```typescript
if (response.status === 429) {
  const retryAfter = response.headers.get('Retry-After');
  console.warn(`Rate limited. Retry after ${retryAfter}s`);
  // Queue request for later
}
```

**3. Server Errors (500+)**

```typescript
if (response.status >= 500) {
  console.error('Service unavailable. Using fallback.');
  // Use cached data or alternative service
}
```

**4. Network Errors**

```typescript
try {
  const response = await fetch(url);
} catch (error) {
  if (error instanceof TypeError) {
    console.error('Network error. Check internet connection.');
    // Enable offline mode
  }
}
```

### Error Response Handling

```typescript
async function safeApiFetch<T>(
  url: string,
  options: RequestInit
): Promise<{ data?: T; error?: string }> {
  try {
    const response = await fetch(url, options);
    
    if (!response.ok) {
      const errorData = await response.json();
      return { error: errorData.message || 'Request failed' };
    }
    
    const data = await response.json();
    return { data };
    
  } catch (error: any) {
    return { error: error.message || 'Unknown error' };
  }
}

// Usage
const { data, error } = await safeApiFetch('/api/openai', {...});
if (error) {
  console.error(error);
  // Show user-friendly message
} else {
  // Use data
}
```

---

## Security Considerations

### API Key Management

1. **Never expose keys client-side**
   ```typescript
   // ❌ Bad - Exposed in browser
   const apiKey = process.env.NEXT_PUBLIC_OPENAI_KEY;
   
   // ✅ Good - Server-side only
   const apiKey = process.env.OPENAI_API_KEY;
   ```

2. **Use environment variables**
   - Store in `.env.local`
   - Add to `.gitignore`
   - Use Vercel environment settings in production

3. **Rotate keys regularly**
   - Monthly rotation recommended
   - Immediate rotation if exposed

4. **Restrict key permissions**
   - Pinata: Only enable `pinFileToIPFS`
   - Infura: Restrict to Sepolia testnet
   - OpenAI: Set spending limits

### Request Validation

```typescript
// Validate requests on server-side
export async function POST(request: Request) {
  // Check origin
  const origin = request.headers.get('origin');
  if (!origin?.includes('chainboard.elpeef.com')) {
    return Response.json({ error: 'Unauthorized' }, { status: 403 });
  }
  
  // Validate input
  const body = await request.json();
  if (!body.text || body.text.length > 10000) {
    return Response.json({ error: 'Invalid input' }, { status: 400 });
  }
  
  // Proceed with API call
  const result = await callExternalAPI(body.text);
  return Response.json(result);
}
```

### CORS Configuration

```typescript
// Next.js API route with CORS
export async function POST(request: Request) {
  // Set CORS headers
  const headers = {
    'Access-Control-Allow-Origin': 'https://chainboard.elpeef.com',
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type',
  };
  
  // Handle preflight
  if (request.method === 'OPTIONS') {
    return new Response(null, { headers });
  }
  
  // Process request
  const result = await processRequest(request);
  return Response.json(result, { headers });
}
```

---

## Monitoring & Debugging

### API Usage Tracking

```typescript
// Log API calls for monitoring
function logApiCall(service: string, endpoint: string, duration: number) {
  console.log(`[API] ${service} - ${endpoint} - ${duration}ms`);
  
  // Optional: Send to analytics
  // analytics.track('api_call', { service, endpoint, duration });
}

// Usage
const startTime = Date.now();
const response = await fetch(url);
const duration = Date.now() - startTime;
logApiCall('OpenAI', '/chat/completions', duration);
```

### Health Checks

```typescript
// Check API health
async function checkApiHealth() {
  const services = [
    { name: 'Infura', test: () => provider.getBlockNumber() },
    { name: 'Pinata', test: () => fetch('https://api.pinata.cloud/data/testAuthentication') },
    { name: 'OpenAI', test: () => fetch('https://api.openai.com/v1/models') },
  ];
  
  for (const service of services) {
    try {
      await service.test();
      console.log(`✅ ${service.name} - Healthy`);
    } catch (error) {
      console.error(`❌ ${service.name} - Down`);
    }
  }
}
```

---

## Support & Resources

### Documentation Links

- **Ethereum**: https://ethereum.org/en/developers/docs/
- **Ethers.js**: https://docs.ethers.org/v6/
- **Infura**: https://docs.infura.io/
- **Pinata**: https://docs.pinata.cloud/
- **OpenAI**: https://platform.openai.com/docs
- **Jitsi**: https://jitsi.github.io/handbook/

### Community Support

- **ChainBoard Support**: support@elpeef.com
- **Discord**: https://discord.com/channels/@khudri_61362
- **Telegram**: https://t.me/khudriakhmad

---

**Last Updated**: January 2026  
**Version**: 1.0.0  
**Maintainer**: @mrbrightsides
