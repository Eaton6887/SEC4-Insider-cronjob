# SEC4-Insider-cronjob

A simple Java bot that runs on GitHub Actions to check for large insider transactions (>500k USD) for specified US stock tickers.

## Features
<img width="1206" height="2622" alt="124c8c6a6c8b33514828527908d2c2d4" src="https://github.com/user-attachments/assets/07e4b9ec-45d8-4c35-ae86-306cd23f52ad" />

- Checks SEC Form 4 filings for the previous day
- Filters for purchase (P) and sale (S) transactions over $500,000
- Sends push notifications via Discord webhook when alerts are found
- Runs daily at 9 AM UTC or manually via workflow dispatch

## Setup

1. Fork this repository.
2. Create a Discord webhook:
   - Open your Discord server and go to `Server Settings` → `Integrations` → `Webhooks`.
   - Click `New Webhook` and select a channel where you want alerts to appear.
   - Copy the webhook URL.
3. Add the webhook URL to GitHub secrets:
   - Open your fork on GitHub and go to `Settings` → `Secrets and variables` → `Actions`.
   - Add a new secret named `DISCORD_WEBHOOK_URL`.
   - Paste the webhook URL as the value.
4. (Optional) Configure repo variables for cron defaults:
   - In `Settings` → `Secrets and variables` → `Variables`, add:
     - `TICKERS`: Comma-separated tickers, e.g. `AAPL,GOOGL,MSFT`
     - `THRESHOLD_USD`: Minimum trade size, e.g. `500000`
     - `LOOKBACK_DAYS`: Lookback days, e.g. `1`
   - If not set, defaults are `AAPL,GOOGL`, `500000`, `1`.
5. Run the workflow manually or wait for the daily schedule:
   - Go to the `Actions` tab, choose `Daily Insider Check`, then `Run workflow`.
   - Enter tickers, threshold, and lookback values as needed.

## Usage

- **Manual run**: Go to Actions tab, select "Daily Insider Check", click "Run workflow", enter your desired tickers, threshold, and lookback days. Defaults are provided.
- **Scheduled**: Runs daily automatically using default values in `./.github/workflows/daily-check.yml`. You need to customise tickers, threshold and lookback to your preferences.
- **Configuration**: 
  - **GitHub secret**: Set `DISCORD_WEBHOOK_URL` in Settings → Secrets and variables → Actions
  - **Ticker format**: `BRKB` or `BRK-B` are supported; `BRK.B` is not supported. Ticker input is case-insensitive.

- Push notifications include ticker, owner, position, action, security, shares, price, and amount on separate lines.
![alt text](image.png)
## Requirements

- Maven
- GitHub Actions

## Notes

- Only checks non-derivative transactions (P/S codes)
- Excludes awards (A) and exercises (M) to avoid RSU-related transactions
- Data from SEC EDGAR, subject to their terms