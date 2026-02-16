# GhostTap — OpenClaw Skill

## Description
Manage your GhostTap anonymous crypto-to-card service through OpenClaw.
Check balances, list wallets and virtual cards, create new ghost cards,
and freeze or unfreeze them — all via natural language from any messaging channel.

## Setup

1. Log into your GhostTap dashboard.
2. Navigate to **OpenClaw** in the sidebar.
3. Click **Generate** to create an API token.
4. Copy the token (it is shown only once).
5. Place this SKILL.md file in `~/.openclaw/skills/ghosttap/SKILL.md`.
6. Replace `<YOUR_TOKEN>` below with your actual token.
## Configuration

```
Webhook URL: https://ghosttap.io/api/openclaw/webhook
Authorization: Bearer <YOUR_TOKEN>
Content-Type: application/json
```

## Available Actions

### Check Balance
Request:
```json
{ "action": "get_balance" }
```
Returns total USD value and per-currency balances for SOL and ETH wallets.

### List Wallets
Request:
```json
{ "action": "list_wallets" }
```
Returns all crypto wallets with addresses and creation dates.

### List Virtual Cards
Request:
```json
{ "action": "list_cards" }
```
Returns all ghost cards with status, last 4 digits, type, and linked wallet.

### Create Virtual Card
Request:
```json
{
  "action": "create_card",
  "params": {
    "name": "Travel Card",
    "cardType": "visa"
  }
}
```
Card types: `visa` (linked to SOL) or `mastercard` (linked to ETH).
Maximum 2 cards per user (one of each type).

### Freeze Card
Request:
```json
{
  "action": "freeze_card",
  "params": { "cardId": "<CARD_ID>" }
}
```

### Unfreeze Card
Request:
```json
{
  "action": "unfreeze_card",
  "params": { "cardId": "<CARD_ID>" }
}
```

## Example Conversations

User: "What's my GhostTap balance?"
→ Calls `get_balance`, responds with SOL/ETH balances and USD totals.

User: "Create a new Visa ghost card called Shopping"
→ Calls `create_card` with name="Shopping" and cardType="visa".

User: "Freeze my card ending in 7291"
→ Calls `list_cards` to find the card ID, then `freeze_card`.

User: "Show me my ghost cards"
→ Calls `list_cards`, responds with card list and statuses.

## Security Notes

- Tokens are hashed server-side; GhostTap never stores raw tokens.
- Each token is scoped to a single user account.
- Revoke tokens anytime from the GhostTap dashboard.
- Maximum 5 active tokens per user.
- All webhook communication should use HTTPS in production.

## Error Handling

- `401`: Invalid or revoked token. Generate a new one from the dashboard.
- `400`: Invalid action or missing parameters. Check the action name and params.
- `404`: Resource not found (e.g., card ID doesn't exist).
- `500`: Server error. Retry or check GhostTap status.
