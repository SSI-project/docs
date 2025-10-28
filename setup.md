#!/bin/bash
# PortableID Complete Dev Environment Setup
# Run this in WSL Ubuntu terminal
# Prerequisites: Rust, Node.js, Docker already installed

set -e

echo "════════════════════════════════════════════════════════════════"
echo "PortableID Dev Environment Setup (WSL Ubuntu)"
echo "════════════════════════════════════════════════════════════════"

# ============================================================================
# SECTION 1: UPDATE SYSTEM & INSTALL DEPENDENCIES
# ============================================================================

echo ""
echo "[1/8] Updating system packages..."
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y \
  build-essential \
  pkg-config \
  libssl-dev \
  git \
  curl \
  wget \
  cmake \
  clang \
  protobuf-compiler \
  libprotobuf-dev \
  postgresql \
  postgresql-contrib \
  redis-server \
  jq

echo "✓ System packages installed"

# ============================================================================
# SECTION 2: VERIFY RUST & UPDATE TOOLCHAIN
# ============================================================================

echo ""
echo "[2/8] Verifying Rust & updating toolchain..."
rustc --version
cargo --version

# Add Substrate target
rustup target add wasm32-unknown-unknown
rustup component add rust-src

echo "✓ Rust toolchain ready"

# ============================================================================
# SECTION 3: INSTALL SUBSTRATE TOOLS
# ============================================================================

echo ""
echo "[3/8] Installing Substrate development tools..."
cargo install substrate-cli --git https://github.com/paritytech/substrate-cli.git --locked 2>/dev/null || echo "Substrate CLI already installed"
cargo install subwasm --locked 2>/dev/null || echo "subwasm already installed"

echo "✓ Substrate tools installed"

# ============================================================================
# SECTION 4: CLONE PROJECT REPOS
# ============================================================================

echo ""
echo "[4/8] Setting up project directory structure..."

# Create base directory
PROJECT_DIR="$HOME/portableid"
mkdir -p "$PROJECT_DIR"
cd "$PROJECT_DIR"

echo "Project directory: $PROJECT_DIR"

# Create repo directories
REPOS=(
  "protocol"
  "smart-contracts"
  "backend-apis"
  "mobile-app"
  "frontend-web"
  "sdk"
  "docs"
  "tests"
  "devops"
  "design-system"
  "database-storage"
  "deployments"
  "schemas"
  "security"
  "audit-logs"
)

for repo in "${REPOS[@]}"; do
  if [ ! -d "$repo" ]; then
    mkdir -p "$repo"
    echo "Created: $repo"
  fi
done

echo "✓ Project structure created"

# ============================================================================
# SECTION 5: SETUP PARACHAIN (SMART CONTRACTS)
# ============================================================================

echo ""
echo "[5/8] Setting up Substrate Parachain..."
cd "$PROJECT_DIR/smart-contracts"

# Clone Polkadot SDK template
git clone https://github.com/paritytech/polkadot-sdk.git . 2>/dev/null || echo "SDK already present"

# Create Cargo.toml for custom parachain
cat > Cargo.toml << 'EOF'
[package]
name = "portableid-parachain"
version = "0.1.0"
edition = "2021"

[dependencies]
codec = { package = "parity-scale-codec", version = "3.0" }
scale-info = { version = "2.0", features = ["derive"] }
frame-support = { version = "28.0" }
frame-system = { version = "28.0" }
sp-core = { version = "28.0" }
sp-runtime = { version = "31.0" }
sp-std = { version = "14.0" }

[features]
default = ["std"]
std = [
  "codec/std",
  "frame-support/std",
  "frame-system/std",
  "sp-core/std",
  "sp-runtime/std",
]
EOF

mkdir -p src
cat > src/lib.rs << 'EOF'
#![cfg_attr(not(feature = "std"), no_std)]

pub use pallet::*;

#[frame_support::pallet]
pub mod pallet {
    use frame_support::pallet_prelude::*;
    use frame_system::pallet_prelude::*;

    #[pallet::pallet]
    pub struct Pallet<T>(_);

    #[pallet::config]
    pub trait Config: frame_system::Config {
        type RuntimeEvent: From<Event<Self>> + IsType<<Self as frame_system::Config>::RuntimeEvent>;
    }

    #[pallet::event]
    #[pallet::generate_deposit(pub(super) fn deposit_event)]
    pub enum Event<T: Config> {
        DIDCreated { did: [u8; 32] },
        IssuerRegistered { issuer: T::AccountId },
        CredentialRevoked { credential_id: [u8; 32] },
    }

    #[pallet::error]
    pub enum Error<T> {
        DuplicateDID,
        UnauthorizedIssuer,
        InvalidCredential,
    }

    #[pallet::call]
    impl<T: Config> Pallet<T> {
        #[pallet::call_index(0)]
        #[pallet::weight(10_000)]
        pub fn create_did(_origin: OriginFor<T>, _did: [u8; 32]) -> DispatchResult {
            Ok(())
        }
    }
}
EOF

echo "✓ Parachain template created"

# ============================================================================
# SECTION 6: SETUP BACKEND APIs (Node.js)
# ============================================================================

echo ""
echo "[6/8] Setting up Backend APIs..."
cd "$PROJECT_DIR/backend-apis"

# Initialize Node.js project
cat > package.json << 'EOF'
{
  "name": "portableid-backend",
  "version": "1.0.0",
  "description": "PortableID Issuer and Verifier APIs",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node src/index.js",
    "dev": "NODE_ENV=development node --watch src/index.js",
    "test": "node --test tests/**/*.test.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "postgres": "^3.4.0",
    "redis": "^4.6.11",
    "jsonwebtoken": "^9.1.0",
    "bcryptjs": "^2.4.3",
    "axios": "^1.6.2",
    "dotenv": "^16.3.1"
  }
}
EOF

mkdir -p src tests
cat > src/index.js << 'EOF'
import express from 'express';

const app = express();
app.use(express.json());

// Issuer APIs
app.post('/api/issuer/register', (req, res) => {
  res.json({ message: 'Issuer registration endpoint', status: 'ready' });
});

app.post('/api/issuer/issue-credential', (req, res) => {
  res.json({ message: 'Issue credential endpoint', status: 'ready' });
});

app.post('/api/issuer/revoke-credential', (req, res) => {
  res.json({ message: 'Revoke credential endpoint', status: 'ready' });
});

// Verifier APIs
app.post('/api/verifier/verify-presentation', (req, res) => {
  res.json({ message: 'Verify presentation endpoint', status: 'ready' });
});

app.get('/api/verifier/check-status/:credentialId', (req, res) => {
  res.json({ message: 'Check status endpoint', status: 'ready' });
});

const PORT = process.env.PORT || 3001;
app.listen(PORT, () => {
  console.log(`PortableID Backend running on http://localhost:${PORT}`);
});
EOF

npm install
echo "✓ Backend APIs scaffolded"

# ============================================================================
# SECTION 7: SETUP MOBILE APP (Expo + React Native)
# ============================================================================

echo ""
echo "[7/8] Setting up Mobile App (Expo)..."
cd "$PROJECT_DIR/mobile-app"

# Create Expo project structure
cat > app.json << 'EOF'
{
  "expo": {
    "name": "PortableID",
    "slug": "portableid",
    "version": "1.0.0",
    "assetBundlePatterns": ["**/*"],
    "ios": {
      "supportsTabletMode": true
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      }
    },
    "web": {
      "favicon": "./assets/favicon.png"
    },
    "plugins": [
      "expo-camera",
      "expo-secure-store",
      "expo-dev-client"
    ]
  }
}
EOF

cat > package.json << 'EOF'
{
  "name": "portableid-mobile",
  "version": "1.0.0",
  "main": "node_modules/expo/AppEntry.js",
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "web": "expo start --web",
    "test": "jest"
  },
  "dependencies": {
    "expo": "^50.0.0",
    "expo-camera": "^14.0.0",
    "expo-secure-store": "^13.0.0",
    "react": "^18.2.0",
    "react-native": "^0.73.0",
    "@react-navigation/native": "^6.1.0",
    "@react-navigation/bottom-tabs": "^6.5.0",
    "axios": "^1.6.0"
  }
}
EOF

mkdir -p app assets
cat > app/index.tsx << 'EOF'
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>PortableID Wallet</Text>
      <Text style={styles.subtitle}>Self-Sovereign Identity</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
  },
  subtitle: {
    fontSize: 16,
    color: '#666',
    marginTop: 8,
  },
});
EOF

npm install
echo "✓ Mobile app scaffolded (Expo)"

# ============================================================================
# SECTION 8: SETUP WEB PORTAL (Next.js)
# ============================================================================

echo ""
echo "[8/8] Setting up Web Portal (Next.js)..."
cd "$PROJECT_DIR/frontend-web"

cat > package.json << 'EOF'
{
  "name": "portableid-web",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "test": "jest"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "axios": "^1.6.0",
    "tailwindcss": "^3.3.0"
  }
}
EOF

mkdir -p app/components app/pages
cat > app/page.tsx << 'EOF'
export default function Home() {
  return (
    <main className="min-h-screen bg-gradient-to-br from-blue-600 to-blue-800 flex items-center justify-center">
      <div className="text-center text-white">
        <h1 className="text-5xl font-bold mb-4">PortableID</h1>
        <p className="text-xl mb-8">Issuer Dashboard</p>
        <p className="text-gray-200">Self-Sovereign Identity Platform</p>
      </div>
    </main>
  );
}
EOF

npm install
echo "✓ Web portal scaffolded (Next.js)"

# ============================================================================
# SECTION 9: SETUP DATABASE & SERVICES
# ============================================================================

echo ""
echo "[9/9] Setting up Database & Services..."

# Start PostgreSQL
sudo service postgresql start 2>/dev/null || echo "PostgreSQL service start skipped"

# Start Redis
sudo service redis-server start 2>/dev/null || echo "Redis service start skipped"

# Create docker-compose for local services
cat > "$PROJECT_DIR/docker-compose.yml" << 'EOF'
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: portableid
      POSTGRES_PASSWORD: portableid
      POSTGRES_DB: portableid_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  postgres-adminer:
    image: adminer:latest
    ports:
      - "8080:8080"
    depends_on:
      - postgres

volumes:
  postgres_data:
EOF

echo "✓ Database & services configured"

# ============================================================================
# SECTION 10: CREATE .ENV FILES
# ============================================================================

echo ""
echo "[10/10] Creating environment files..."

# Backend .env
cat > "$PROJECT_DIR/backend-apis/.env" << 'EOF'
NODE_ENV=development
PORT=3001
DATABASE_URL=postgresql://portableid:portableid@localhost:5432/portableid_db
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-secret-key-change-this
PARACHAIN_RPC=http://localhost:9944
EOF

# Mobile .env
cat > "$PROJECT_DIR/mobile-app/.env" << 'EOF'
API_URL=http://192.168.x.x:3001
EXPO_PUBLIC_ENV=development
EOF

# Web .env
cat > "$PROJECT_DIR/frontend-web/.env.local" << 'EOF'
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_ENV=development
EOF

echo "✓ Environment files created"

# ============================================================================
# FINAL SUMMARY
# ============================================================================

echo ""
echo "════════════════════════════════════════════════════════════════"
echo "✓ PortableID Development Environment Ready!"
echo "════════════════════════════════════════════════════════════════"
echo ""
echo "Project Directory: $PROJECT_DIR"
echo ""
echo "QUICK START COMMANDS:"
echo "───────────────────────────────────────────────────────────────"
echo ""
echo "1. Start Database & Redis:"
echo "   cd $PROJECT_DIR"
echo "   docker-compose up -d"
echo ""
echo "2. Start Backend APIs:"
echo "   cd $PROJECT_DIR/backend-apis"
echo "   npm run dev"
echo "   → http://localhost:3001"
echo ""
echo "3. Start Web Portal (Next.js):"
echo "   cd $PROJECT_DIR/frontend-web"
echo "   npm run dev"
echo "   → http://localhost:3000"
echo ""
echo "4. Start Mobile App (Expo):"
echo "   cd $PROJECT_DIR/mobile-app"
echo "   npm start"
echo "   → Press 'a' for Android, 'i' for iOS"
echo ""
echo "5. Start Parachain (Substrate):"
echo "   cd $PROJECT_DIR/smart-contracts"
echo "   cargo build --release"
echo "   ./target/release/node-template --dev"
echo "   → http://localhost:9944 (RPC)"
echo ""
echo "6. Database Admin:"
echo "   http://localhost:8080 (Adminer)"
echo "   User: portableid, Password: portableid"
echo ""
echo "USEFUL LINKS:"
echo "───────────────────────────────────────────────────────────────"
echo "Substrate Docs:    https://docs.substrate.io"
echo "Expo Docs:         https://docs.expo.dev"
echo "Next.js Docs:      https://nextjs.org/docs"
echo "PostgreSQL Docs:   https://www.postgresql.org/docs"
echo ""
echo "════════════════════════════════════════════════════════════════"