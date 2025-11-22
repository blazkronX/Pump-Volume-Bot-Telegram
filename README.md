# 🤖 Pump.fun Volume Bot with Telegram Control

A sophisticated Solana volume bot designed for Pump.fun tokens, featuring complete Telegram bot integration for remote management and monitoring. This bot automates token trading to generate volume through coordinated buy/sell operations across multiple wallets.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.5.4-blue.svg)](https://www.typescriptlang.org/)
[![Solana](https://img.shields.io/badge/Solana-Web3.js-green.svg)](https://solana.com/)

## ⚡ Features

- **🎮 Telegram Bot Control**: Complete remote management via Telegram commands
- **🔐 Auto-Authorization**: First user to interact is automatically authorized
- **💰 Multi-Wallet Management**: Automatically creates and manages sub-wallets
- **🔄 Automated Buy/Sell Cycles**: Coordinated volume generation with configurable parameters
- **📊 Real-time Status Updates**: Monitor bot operations directly from Telegram
- **⚙️ Dynamic Configuration**: Adjust SOL amount, slippage, and sleep time on-the-fly
- **🚀 Jito Bundle Integration**: MEV-protected transactions with bundle support
- **🔍 Address Lookup Tables (LUT)**: Optimized transaction size and cost efficiency
- **💸 Token Selling**: Sell all tokens from sub-wallets with one command
- **📥 SOL Collection**: Gather SOL from all sub-wallets back to main wallet
- **🛡️ Rate Limiting**: Built-in Telegram API rate limit handling
- **⚠️ Error Recovery**: Automatic retry mechanisms and comprehensive error handling

## 📋 Requirements

- **Node.js**: v16.x or higher
- **Yarn** or **npm**: Package manager
- **Solana RPC**: High-quality RPC endpoint (recommended: Helius, QuickNode, or Alchemy)
- **Telegram Bot Token**: Create via [@BotFather](https://t.me/BotFather)
- **Funded Solana Wallet**: Main wallet with SOL for operations

## 🚀 Installation

### 1. Install Dependencies

```bash
# Using npm
npm install

# Using yarn
yarn install
```

### 3. Configure Environment Variables

Create a `.env` file in the root directory:

```env
# REQUIRED: Solana RPC endpoint (use a premium RPC for best results)
RPC_URL=https://your-solana-rpc-endpoint.com

# REQUIRED: Your main wallet's private key (base58 encoded)
PRIVATE_KEY=your_wallet_private_key_base58

# REQUIRED: Telegram bot token from @BotFather
TELEGRAM_BOT_TOKEN=your_telegram_bot_token

# OPTIONAL: Manual user ID authorization (if not using auto-capture)
# TELEGRAM_ALLOWED_USER_IDS=123456789

# OPTIONAL: Jito tip amount in lamports (default: 1000000 = 0.001 SOL)
JITO_TIP_AMOUNT_LAMPORTS=1000000
```

### 4. Obtain Required Credentials

#### Get Solana RPC URL:
- [Helius](https://helius.dev/) - Recommended
- [QuickNode](https://www.quicknode.com/)
- [Alchemy](https://www.alchemy.com/)
- Free option: `https://api.mainnet-beta.solana.com` (not recommended for production)

#### Get Telegram Bot Token:
1. Open Telegram and search for [@BotFather](https://t.me/BotFather)
2. Send `/newbot` and follow the prompts
3. Copy the bot token provided

#### Get Your Wallet Private Key:
```bash
# If using Phantom or other wallets, export your private key
# Convert to base58 if needed using solana-keygen
solana-keygen pubkey <path-to-keypair.json>
```

## 🎯 Usage

### Start the Telegram Bot

```bash
# Using npm
npm run bot

# Using yarn
yarn bot

# Using ts-node directly
ts-node bot.ts
```

### Telegram Bot Commands

Once the bot is running, interact with it on Telegram:

#### `/help`
Display comprehensive help message with all available commands and features.

#### `/settings`
Access the main control panel with interactive buttons:
- **💰 SOL/Swap**: Set SOL amount each sub-wallet uses per buy/sell cycle
- **💧 Slippage**: Configure max price deviation (0.1% - 50%)
- **🔑 Token**: Set the Pump.fun token contract address
- **⏱️ Sleep**: Set pause duration between swap cycles (milliseconds)
- **▶️ Start Bot**: Initialize and start volume generation
- **⏹️ Stop Bot**: Halt the volume generation loop
- **💸 Sell All Tokens**: Sell all target tokens from sub-wallets
- **📥 Collect All SOL**: Transfer SOL from sub-wallets to main wallet

#### `/status`
View current bot configuration and operational status:
- Running state (Active/Idle)
- Target token address
- SOL per swap cycle
- Slippage percentage
- Cycle sleep time
- Main wallet balance
- Authorized user ID

### Typical Workflow

1. **Start the Bot**
   ```bash
   npm run bot
   ```

2. **Authorize Yourself** (First Time)
   - Send any command (e.g., `/help`) to the bot on Telegram
   - You'll be automatically authorized as the first user

3. **Configure Settings**
   - Send `/settings` to access the control panel
   - Set your target token address (Pump.fun token CA)
   - Configure SOL amount per swap (e.g., 0.005 SOL)
   - Set slippage tolerance (e.g., 1% = 1.0)
   - Set sleep time between cycles (e.g., 5000ms)

4. **Start Volume Generation**
   - Click "▶️ Bot Stopped (Start)" in settings
   - The bot will:
     - Create/load sub-wallets (if needed)
     - Create/load Address Lookup Table
     - Distribute SOL to sub-wallets
     - Begin automated buy/sell cycles

5. **Monitor Operations**
   - Use `/status` to check current state
   - Telegram notifications for each cycle completion
   - Console logs for detailed transaction info

6. **Stop and Cleanup** (When Done)
   - Click "⏹️ Bot Running (Stop)" to halt operations
   - Use "💸 Sell All Tokens" to liquidate remaining tokens
   - Use "📥 Collect All SOL" to gather funds back to main wallet

## 🏗️ Architecture

### Core Components

#### **bot.ts** - Entry Point
Initializes the Telegram controller and starts the bot service.

#### **index.ts** (PumpfunVbot Class)
Main bot logic containing:
- `getPumpData()`: Fetches bonding curve data from Pump.fun
- `createWallets()`: Generates new sub-wallets
- `loadWallets()`: Loads existing wallets from storage
- `createLUT()`: Creates Address Lookup Table for optimization
- `extendLUT()`: Adds addresses to the lookup table
- `distributeSOL()`: Sends SOL to sub-wallets
- `swap()`: Executes buy/sell cycles across wallets
- `sellAllTokensFromWallets()`: Liquidates all token holdings
- `collectSOL()`: Retrieves SOL from sub-wallets

#### **src/telegram.ts** - Telegram Controller
Handles all Telegram bot interactions:
- Command routing and authorization
- Interactive keyboard management
- Rate limiting and retry logic
- User configuration management
- Bot lifecycle control

#### **src/config.ts** - Configuration
- Environment variable management
- Keypair initialization
- Auto-authorization system
- Default parameter settings

#### **src/constants.ts** - Solana Constants
- Pump.fun program addresses
- System program IDs
- Token program references

#### **src/jito.bundle.ts** - Jito Integration
- Bundle creation and submission
- Transaction status monitoring
- MEV protection

#### **src/utils.ts** - Utility Functions
- Buffer operations
- Array chunking
- Time utilities

### Data Storage

The bot creates and manages these files:

- **`wallets.json`**: Sub-wallet private keys (read-only for security)
- **`lut.json`**: Address Lookup Table address
- **`telegram_user_id.json`**: Authorized Telegram user ID
- **`.env`**: Environment configuration (not committed to git)

## ⚙️ Configuration Details

### Default Parameters (src/config.ts)

```typescript
// Amount of SOL to distribute to each sub-wallet
DefaultDistributeAmountLamports = 0.004 SOL

// Jito tip amount for MEV protection
DefaultJitoTipAmountLamports = 1,000,000 lamports (0.001 SOL)

// Default slippage tolerance
DefaultSlippage = 0.5 (50%)

// Default token address (placeholder - set via Telegram)
DefaultCA = 'YOUR_DEFAULT_PUMP_FUN_TOKEN_CA_HERE'
```

### Adjustable via Telegram

- **SOL Amount**: 0.00004 - 100 SOL per sub-wallet
- **Slippage**: 0.1% - 50%
- **Sleep Time**: ≥1000ms between cycles
- **Token Address**: Any valid Pump.fun token

## 🔒 Security Considerations

### 🚨 Critical Security Practices

1. **Never Share Your Private Keys**
   - Keep your `.env` file secure and never commit it to version control
   - The `.gitignore` file already excludes `.env`, `wallets.json`, and user data

2. **Secure Your Telegram Bot**
   - Only the first user to interact is automatically authorized
   - To reset authorization, delete `telegram_user_id.json` and restart the bot
   - Keep your bot token confidential

3. **Wallet Security**
   - `wallets.json` is automatically set to read-only (400 permissions on Unix)
   - Store backups of `wallets.json` securely if you have funds in sub-wallets
   - Never share `wallets.json` publicly

4. **RPC Endpoint**
   - Use a private/paid RPC endpoint for better reliability and security
   - Free public RPCs may be rate-limited or unreliable

5. **Fund Management**
   - Start with small amounts for testing
   - Monitor operations closely
   - Be aware of Solana network fees and Jito tips

### Permission Recommendations

```bash
# Set restrictive permissions on sensitive files (Unix/Linux/Mac)
chmod 400 wallets.json
chmod 400 .env
chmod 600 telegram_user_id.json
```

## 🐛 Troubleshooting

### Bot Won't Start

**Issue**: Bot fails to start or crashes immediately

**Solutions**:
- Verify all required environment variables in `.env`
- Check RPC URL is valid and accessible
- Ensure private key is correctly formatted (base58)
- Verify Telegram bot token is correct

### Insufficient Balance Errors

**Issue**: "Insufficient SOL balance" messages

**Solutions**:
- Ensure main wallet has sufficient SOL
- Account for transaction fees and Jito tips
- For 10 wallets with 0.004 SOL each: ~0.06 SOL + fees needed

### Transaction Failures

**Issue**: Transactions fail or timeout

**Solutions**:
- Switch to a premium RPC endpoint (Helius, QuickNode)
- Increase Jito tip amount for higher priority
- Reduce number of wallets in chunks
- Check Solana network status

### Telegram Rate Limiting

**Issue**: "Rate limited by Telegram" warnings

**Solutions**:
- The bot automatically handles rate limits with retries
- Increase `TELEGRAM_RATE_LIMIT_DELAY` in `telegram.ts` if needed
- Avoid sending too many commands rapidly

### LUT Issues

**Issue**: Lookup Table creation or extension fails

**Solutions**:
- Ensure main wallet has sufficient SOL (~0.005 SOL)
- Wait 25-30 seconds after LUT creation before operations
- Delete `lut.json` and recreate if corrupted
- Verify RPC connection is stable

### Token Not Found

**Issue**: "Invalid token address" or "Token account does not exist"

**Solutions**:
- Verify the token address is correct
- Ensure it's a valid Pump.fun token
- Check the token hasn't migrated to Raydium
- Try setting the token again in `/settings`

### Authorization Issues

**Issue**: "You are not authorized" messages

**Solutions**:
- Check `telegram_user_id.json` contains your correct user ID
- To reset: delete the file and restart the bot
- Send any command to re-authorize as first user
- Verify you're messaging the correct bot

## 📊 Performance Optimization

### Tips for Best Results

1. **Use Premium RPC**: Helius, QuickNode, or Alchemy for reliability
2. **Adjust Chunk Sizes**: Reduce wallet chunks if transactions are too large
3. **Optimize Sleep Time**: Balance between volume generation and transaction success
4. **Monitor Slippage**: Adjust based on token volatility
5. **Jito Tips**: Increase for higher priority during network congestion

### Transaction Size Limits

- Maximum transaction size: 1232 bytes
- Bot automatically validates and skips oversized transactions
- Reduce wallets per chunk if hitting size limits

## 🔧 Development

### Build the Project

```bash
npm run build
```

### Run Linter

```bash
npm run lint
```

### Run Directly (Development)

```bash
# Run the main bot
npm start

# Run Telegram controller
npm run bot
```

## 📝 Scripts Reference

- `npm start` - Run main volume bot (index.ts)
- `npm run bot` - Run Telegram controller (bot.ts)
- `npm run build` - Compile TypeScript to JavaScript
- `npm run lint` - Run ESLint on TypeScript files

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## ⚖️ Legal Disclaimer

**IMPORTANT**: This bot is provided for educational purposes only.

- Trading cryptocurrencies carries significant risk
- Volume generation may be against exchange terms of service
- Users are responsible for compliance with local regulations
- The authors assume no liability for financial losses
- Use at your own risk and discretion

**Always comply with applicable laws and regulations in your jurisdiction.**

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Solana Web3.js](https://github.com/solana-labs/solana-web3.js) - Solana JavaScript SDK
- [node-telegram-bot-api](https://github.com/yagop/node-telegram-bot-api) - Telegram Bot API
- [Jito Labs](https://www.jito.wtf/) - MEV protection and bundle services
- [Pump.fun](https://pump.fun/) - Token platform

## 📞 Support

For issues, questions, or contributions:

- Open an issue on GitHub
- Review existing issues and discussions
- Check the troubleshooting section above

## 🗺️ Roadmap

Potential future enhancements:

- [ ] Multi-token support (manage multiple tokens simultaneously)
- [ ] Advanced trading strategies (TWAP, VWAP)
- [ ] Web dashboard for monitoring
- [ ] Database integration for persistent storage
- [ ] Webhook notifications
- [ ] Profit/loss tracking and analytics
- [ ] Support for other Solana DEXes

---

**⚠️ Use Responsibly**: This tool automates trading operations. Always start with small amounts, monitor closely, and understand the risks involved.

**Made with ❤️ for the Solana community**

