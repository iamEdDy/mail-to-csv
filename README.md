# Gmail Sent Emails to CSV Exporter

A fast and efficient Python script to export sent Gmail emails to CSV format using the Gmail API, with advanced wallet data extraction capabilities.

## Features

- ✅ Export sent emails to CSV format
- ✅ **Real-time incremental CSV writing** - See progress as emails are processed
- ✅ Fast pagination and bulk processing
- ✅ Proper authentication with OAuth2
- ✅ Extract email headers, body, and metadata
- ✅ Customizable search queries
- ✅ Error handling and logging
- ✅ Support for large email volumes
- ✅ **Advanced wallet data extraction** - Extract specific fields like Phrase, Keystore, Keystore Pass, Private Key
- ✅ **Multi-line data support** - Handle complex data patterns and arrays
- ✅ **Flexible field detection** - Works with different field orders and formats

## Prerequisites

- Python 3.7 or higher
- Gmail account
- Google Cloud Project with Gmail API enabled

## Setup Instructions

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Set up Google Cloud Project

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Gmail API:
   - Go to "APIs & Services" > "Library"
   - Search for "Gmail API"
   - Click "Enable"

### 3. Create OAuth 2.0 Credentials

**Option A: Desktop Application (Recommended)**

1. Go to "APIs & Services" > "Credentials"
2. Click "Create Credentials" > "OAuth 2.0 Client IDs"
3. **Important**: Choose "Desktop application" as the application type
4. Give it a name (e.g., "Gmail CSV Exporter")
5. Click "Create"
6. Download the credentials file and rename it to `credentials.json`
7. Place `credentials.json` in the same directory as the script

**Option B: Web Application (Alternative)**

If you're still getting store ID requirements:

1. Go to "APIs & Services" > "Credentials"
2. Click "Create Credentials" > "OAuth 2.0 Client IDs"
3. Choose "Web application" as the application type
4. Give it a name (e.g., "Gmail CSV Exporter")
5. Add these Authorized redirect URIs:
   - `http://localhost:8080/`
   - `http://localhost:8090/`
   - `http://localhost:9000/`
6. Click "Create"
7. Download the credentials file and rename it to `credentials.json`

**Option C: Using Service Account (Advanced)**

For automated/headless usage:

1. Go to "APIs & Services" > "Credentials"
2. Click "Create Credentials" > "Service Account"
3. Fill in the service account details
4. Grant "Gmail API" permissions
5. Create and download a JSON key
6. Rename to `credentials.json`

### 4. Configure OAuth Consent Screen

If you haven't set up the OAuth consent screen:

1. Go to "APIs & Services" > "OAuth consent screen"
2. Choose "External" user type
3. Fill in the required information:
   - App name: "Gmail CSV Exporter"
   - User support email: Your email
   - Developer contact information: Your email
4. Add scopes: `https://www.googleapis.com/auth/gmail.readonly`
5. Add test users (your Gmail address)
6. Save and continue

### 5. First Run Authentication

On the first run, the script will:
1. Open a browser window for authentication
2. Ask you to sign in to your Google account
3. Grant permission to access your Gmail
4. Save the authentication token for future use

## Usage

### Basic Email Export

Export the most recent 1000 sent emails:

```bash
python gmail_to_csv.py
```

### Advanced Email Export

Export with custom parameters:

```bash
python gmail_to_csv.py \
    --output my_emails.csv \
    --max-results 5000 \
    --query "in:sent after:2023/01/01"
```

### Real-time Export (Recommended)

Export with live progress updates:

```bash
python gmail_to_csv.py \
    --query "in:sent \"Phrase\"" \
    --output emails_with_phrase.csv \
    --max-results 10000
```

### Wallet Data Extraction

Extract specific wallet-related fields from emails:

```bash
# Analyze the structure first
python extract_wallet_data.py emails_with_phrase.csv --analyze

# Extract wallet data to new CSV
python extract_wallet_data.py emails_with_phrase.csv --output wallet_data.csv
```

### Complete Workflow Example

```bash
# 1. Export emails containing "Phrase"
python gmail_to_csv.py --query "in:sent \"Phrase\"" --output emails_with_phrase.csv

# 2. Extract wallet data from the exported emails
python extract_wallet_data.py emails_with_phrase.csv --output wallet_data.csv

# 3. View the extracted data
head -10 wallet_data.csv
```

## Command Line Options

### Gmail Export Options

- `--credentials`: Path to credentials.json file (default: credentials.json)
- `--output`: Output CSV file path (default: sent_emails.csv)
- `--max-results`: Maximum number of emails to export (default: 1000)
- `--query`: Gmail search query (default: in:sent)
- `--no-incremental`: Disable real-time CSV writing (legacy mode)

### Wallet Data Extraction Options

- `input`: Input CSV file path (required)
- `--output, -o`: Output CSV file path (default: wallet_data.csv)
- `--analyze, -a`: Analyze CSV structure without extracting

## Gmail Search Query Examples

- `in:sent after:2023/01/01` - Sent emails after January 1, 2023
- `in:sent to:example@gmail.com` - Sent emails to specific address
- `in:sent subject:meeting` - Sent emails with "meeting" in subject
- `in:sent has:attachment` - Sent emails with attachments
- `in:sent is:important` - Important sent emails
- `in:sent "Phrase"` - Sent emails containing the word "Phrase"

## Output Formats

### Email Export CSV

The email CSV file contains the following columns:

| Column | Description |
|--------|-------------|
| id | Gmail message ID |
| thread_id | Gmail thread ID |
| subject | Email subject |
| from | Sender email address |
| to | Recipient email address |
| cc | CC recipients |
| date | Email date (YYYY-MM-DD HH:MM:SS) |
| snippet | Email preview snippet |
| body | Full email body |
| labels | Gmail labels |

### Wallet Data CSV

The wallet data CSV contains extracted fields:

| Column | Description |
|--------|-------------|
| email_id | Gmail message ID |
| subject | Email subject |
| from | Sender email address |
| date | Email date |
| phrase | Extracted phrase data |
| keystore | Extracted keystore data |
| keystore_pass | Extracted keystore password |
| private_key | Extracted private key |

## Performance Tips

1. **Use real-time export** for large datasets:
   ```bash
   python gmail_to_csv.py --query "in:sent \"Phrase\"" --max-results 10000
   ```

2. **Use specific date ranges** to reduce processing time:
   ```bash
   python gmail_to_csv.py --query "in:sent after:2023/01/01 before:2023/12/31"
   ```

3. **Limit results** for testing:
   ```bash
   python gmail_to_csv.py --max-results 100
   ```

4. **Use filters** to get specific emails:
   ```bash
   python gmail_to_csv.py --query "in:sent has:attachment"
   ```

## Troubleshooting

### Authentication Issues

1. **"Credentials file not found"**
   - Ensure `credentials.json` is in the same directory as the script
   - Download fresh credentials from Google Cloud Console

2. **"Invalid credentials"**
   - Delete `token.json` and re-authenticate
   - Check that your Google Cloud Project has Gmail API enabled

3. **"Access denied"**
   - Ensure you're using the correct Google account
   - Check that the OAuth consent screen is configured properly

4. **"Store ID required"**
   - Make sure you selected "Desktop application" not "Android" or "iOS"
   - If using "Web application", add localhost redirect URIs

5. **"OAuth consent screen not configured"**
   - Go to "APIs & Services" > "OAuth consent screen"
   - Configure the consent screen with required information
   - Add your email as a test user

6. **"App is currently being tested"**
   - Add your email as a test user in OAuth consent screen, OR
   - Click "Publish App" to make it public

### API Quota Issues

- Gmail API has daily quotas (1 billion queries per day per user)
- If you hit quota limits, wait 24 hours or request quota increase

### Memory Issues

- For large exports (>10,000 emails), consider using smaller batches
- Use specific date ranges to reduce memory usage

### Data Extraction Issues

- Use `--analyze` flag to check data structure before extraction
- Check for multi-line data or special characters
- Verify field names match exactly (case-sensitive)

## Security Notes

- Keep `credentials.json` and `token.json` secure and don't share them
- The script only requests read-only access to Gmail
- Tokens are stored locally and can be deleted to revoke access
- Extracted wallet data should be handled securely

## Example Output

### Email Export
```csv
id,thread_id,subject,from,to,cc,date,snippet,body,labels
18c1234567890abcdef,18c1234567890abcdef,"Meeting Tomorrow","you@gmail.com","colleague@company.com","",2023-12-01 14:30:00,"Hi, just confirming our meeting tomorrow...","Hi, just confirming our meeting tomorrow at 2 PM. Best regards",SENT
```

### Wallet Data Extraction
```csv
email_id,subject,from,date,phrase,keystore,keystore_pass,private_key
1978cc9de64a9d24,"New message from Solaxy live","Solaxy live <1mywalletbackup@gmail.com>",2025-06-20 03:02:09,tresdt,,,
1978cc5e7800fa5b,"New message from cedardao live","cedardao live <1mywalletbackup@gmail.com>",2025-06-20 02:57:49,,bjbbkk,b jbjkb,
```

## Files in This Project

- `gmail_to_csv.py` - Main Gmail export script
- `extract_wallet_data.py` - Wallet data extraction script
- `setup.py` - Automated setup script
- `fix_oauth.py` - OAuth troubleshooting helper
- `example_usage.py` - Usage examples
- `requirements.txt` - Python dependencies
- `README.md` - This documentation
- `TROUBLESHOOTING.md` - Detailed troubleshooting guide

## License

This project is open source and available under the MIT License.

## Contributing

Feel free to submit issues and enhancement requests! 