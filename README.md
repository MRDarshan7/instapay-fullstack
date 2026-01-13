⚡ InstaPay — Gasless Stablecoin Transfers on Shardeum

InstaPay is a full-stack Web3 application that enables gasless USDC transfers on the Shardeum EVM Testnet.
Users can send stablecoins without holding ETH — a relayer covers gas fees, while INCO FHE ensures privacy and a confidential risk engine protects users from malicious destinations.

This repository represents the entire ecosystem:

React Frontend 
        ↓
Express Backend (Risk Engine)
        ↓
Relayer Wallet (Pays Gas)
        ↓
MockUSDC Contract on Shardeum

✨ Features

🔌 MetaMask wallet integration

🌐 Automatic Shardeum network enforcement

🛂 ERC20 approval flow

🧠 Confidential risk assessment (LOW / MEDIUM / HIGH)

⛽ Gasless transfers via relayer

🔁 Retry-safe backend with RPC failure handling

🎉 Modern UI with animations, overlays & confetti

🚀 Production-ready for Vercel + Render

🧱 Project Structure
instapay/
├─ backend/
│  ├─ package.json
│  └─ src/
│     ├─ index.js
│     ├─ chains.js
│     ├─ config.js
│     ├─ fhe.js
│     ├─ abi/MockUSDC.js
│     ├─ routes/transfer.js
│     └─ services/
│        ├─ provider.js
│        ├─ relayer.js
│        ├─ riskAssessment.js
│        └─ transferService.js
│
└─ frontend/
   ├─ package.json
   ├─ tailwind.config.js
   ├─ postcss.config.js
   ├─ public/
   └─ src/
      ├─ App.js
      ├─ fhe.js
      ├─ index.js
      └─ styles

🔐 Architecture
Frontend (React + Tailwind)

User connects MetaMask

App ensures Shardeum Testnet

User approves USDC to the relayer address

On “Send”:

recipient and amount are encrypted via INCO FHE

POST request sent to backend:

{
  "sender": "0xUser",
  "recipient": "0xEncrypted",
  "amount": "0xEncrypted"
}

Backend (Express + Ethers)

Decrypts fields using INCO

Runs confidential risk assessment

Blocks HIGH-RISK destinations

Uses relayer wallet to execute:

transferFrom(sender, recipient, amount)


Returns transaction hash & explorer link

🌍 Supported Network

Currently hard-pinned to:

Shardeum EVM Testnet

Chain ID: 8119

USDC (Mock): 0x1D782Be54c51c95c60088Ea8f7069b51F8E84142

Explorer: https://explorer-mezame.shardeum.org

🔧 Environment Variables
Backend (backend/.env)
PORT=3000
RELAYER_PRIVATE_KEY=0xYOUR_RELAYER_PRIVATE_KEY
SHARDEUM_RPC=https://your-shardeum-rpc

Frontend (frontend/.env)
REACT_APP_API_URL=https://your-backend.onrender.com

📡 API
POST /api/send

Request:

{
  "sender": "0xUserAddress",
  "recipient": "0xEncryptedRecipient",
  "amount": "0xEncryptedAmount"
}


Success Response:

{
  "success": true,
  "network": "Shardeum EVM Testnet",
  "chainId": 8119,
  "txHash": "0x...",
  "etherscanTx": "https://explorer-mezame.shardeum.org/tx/0x...",
  "riskAssessment": {
    "level": "LOW",
    "checked": true
  }
}


Blocked (High Risk):

{
  "error": "Transaction blocked by security layer",
  "riskLevel": "HIGH",
  "reason": "Address is flagged in confidential blacklist"
}

🧠 Risk Engine

The backend runs a confidential risk layer:

HIGH – Blacklisted addresses → ❌ Blocked

MEDIUM – Suspicious patterns → ⚠ Allowed with warning

LOW – Safe → ✅ Proceed

This logic is fully server-side and protected behind FHE boundaries.

🚀 Deployment
Backend (Render)

Create a new Web Service

Set root to backend/

Build Command:

npm install


Start Command:

npm start


Add environment variables

Health endpoint:

GET /health → { "status": "ok" }

Frontend (Vercel)

Import frontend/ directory

Add environment variable:

REACT_APP_API_URL=https://your-backend.onrender.com


Deploy

🛡️ Security Notes

Users never expose private keys

Backend never stores user secrets

Relayer only operates on approved allowances

INCO FHE protects sensitive fields

High-risk addresses are blocked server-side

🧪 Local Development (Optional)
# Backend
cd backend
npm install
npm start

# Frontend
cd frontend
npm install
npm start

🧭 Roadmap

Multi-chain routing (cheapest gas)

On-chain encrypted blacklist

Relayer balance monitoring

Transaction history dashboard

Account abstraction support

❤️ Credits

Built with:

React + Tailwind

Ethers v6

Express

INCO SDK

Shardeum

InstaPay — Send stablecoins without gas, friction, or fear.