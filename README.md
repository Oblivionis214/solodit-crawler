# Solodit Findings Crawler

Automatically fetches security audit findings from [Solodit](https://solodit.cyfrin.io/) API and stores them in Excel files.

## Features

- **Incremental updates**: Only fetches new findings after initial sync
- **Split by severity**: HIGH/MEDIUM and LOW/GAS findings stored separately
- **Rate limit handling**: Respects API rate limits (20 req/60s)
- **Daily automation**: GitHub Action runs daily at 2 AM UTC

## Files

- `high_medium_findings.xlsx` - HIGH and MEDIUM severity findings
- `low_gas_findings.xlsx` - LOW and GAS severity findings
- `state.json` - Tracks last fetched ID for incremental updates

## Setup

### 1. Fork this repository

### 2. Add API Key secret

1. Go to repository **Settings** > **Secrets and variables** > **Actions**
2. Click **New repository secret**
3. Name: `SOLODIT_API_KEY`
4. Value: Your Solodit API key (get it from [solodit.cyfrin.io](https://solodit.cyfrin.io/))

### 3. Enable GitHub Actions

The workflow will run automatically daily, or you can trigger it manually from the **Actions** tab.

## Local Development

```bash
# Install dependencies
pip install -r requirements.txt

# Set API key
export SOLODIT_API_KEY=sk_your_api_key_here

# Run crawler
python fetch_solodit.py
```

## Data Fields

Each finding includes:
- id, slug, title, impact
- quality_score, general_score (rarity)
- firm_name, protocol_name
- content (full markdown), summary
- tags, finders, finders_count
- source_link, github_link, pdf_link
- contest_link, contest_prize_txt
- report_date

## API Limits

- Rate limit: 20 requests per 60 seconds
- Max page size: 100 findings per request
- Initial fetch: ~26 minutes for ~50k findings
- Daily incremental: ~1-5 minutes

## License

MIT
