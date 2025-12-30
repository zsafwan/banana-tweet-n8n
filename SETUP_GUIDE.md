# Tweet Categorization Workflow - Setup Guide

## Overview
This n8n workflow automatically processes tweet links in your Notion database, extracts content, uses Claude AI for categorization and prompt extraction, and updates the database with structured information.

**Key feature**: The workflow intelligently extracts actual nano banana pro prompts from tweets when they're present, making it easy to build a searchable library of working prompts.

## Prerequisites

1. **n8n instance** (cloud or self-hosted)
2. **Notion account** with API access
3. **Anthropic API key** (for Claude)
4. **Twitter/X links** in your Notion database

## Step 1: Set Up Your Notion Database

Create a Notion database with these properties:

| Property Name | Type | Description |
|--------------|------|-------------|
| Name | Title | Tweet title/identifier |
| URL | URL | The tweet link |
| Category | Select | Main category (create options below) |
| Tags | Multi-select | Multiple tags |
| Description | Text | AI-generated description |
| Prompt | Text | Extracted nano banana pro prompt |
| Tweet Content | Text | Full tweet text |
| Author | Text | Tweet author username |
| Processing Status | Select | Pending/Processed/Error |
| Date Processed | Date | When it was processed |
| Date Added | Created time | Auto-filled by Notion |

### Create Category Options:
- Technique
- Use Case
- Tips
- Examples
- Tutorial
- Inspiration
- Discussion
- Uncategorized

### Create Initial Tag Options (will grow over time):
- prompt-engineering
- nano-banana
- best-practices
- advanced
- beginner-friendly
- needs-review

## Step 2: Import the Workflow to n8n

1. Copy the `tweet-categorization-workflow.json` file
2. In n8n, click the "..." menu → "Import from File"
3. Paste the JSON or upload the file
4. Click "Import"

## Step 3: Configure Credentials

### Notion Credentials:
1. Go to https://www.notion.so/my-integrations
2. Create a new integration
3. Copy the "Internal Integration Token"
4. In n8n, add Notion credentials with this token
5. **Important**: Share your Notion database with the integration
   - Open your database in Notion
   - Click "..." → "Add connections"
   - Select your integration

### Anthropic API Credentials:
1. Get your API key from https://console.anthropic.com/
2. In n8n, add Anthropic API credentials
3. Paste your API key

## Step 4: Configure the Workflow Nodes

### Notion Trigger Node:
1. Select your Notion credentials
2. Choose your database from the dropdown
3. The filter is already set to trigger on "Processing Status = Pending"

### Fetch Tweet Content Node:
- Uses vxtwitter.com API (free, no auth needed)
- Automatically extracts tweet ID from URL
- **Alternative**: If vxtwitter stops working, replace with:
  - Twitter API (requires developer account)
  - Nitter scraper
  - Apify Twitter scraper

### Claude Analysis Node:
- Uses Claude Haiku 4.5 (cost-effective)
- Temperature set to 0.3 for consistent categorization
- Max tokens: 500

### Parse Response Node:
- Handles JSON parsing with error fallback
- Prepares data for Notion update

### Update Notion Node:
- Updates all fields in your database
- Sets status to "Processed"

## Step 5: Test the Workflow

1. Add a tweet link to your Notion database
2. Set "Processing Status" to "Pending"
3. In n8n, click "Execute Workflow"
4. Check if the database was updated correctly

## Step 6: Activate for Automatic Processing

1. Click the toggle at the top to activate
2. New tweets with "Pending" status will be processed automatically
3. Check frequency: Every minute (adjust in Notion Trigger if needed)

## Usage Workflow

1. **Add tweets to Notion**: Paste tweet URLs, leave Processing Status as "Pending"
2. **Automatic processing**: n8n checks every minute and processes pending items
3. **Review results**: Check the categorization, edit if needed
4. **Search & filter**: Use Notion's database views to find tweets by category/tags

## Customization Options

### Adjust Categories:
Edit the Claude Analysis prompt to change available categories:
```
\"category\": \"<one of: YourCategory1|YourCategory2|...>\"
```

### Change Processing Frequency:
In Notion Trigger → Poll Times → Change from "everyMinute" to your preference

### Improve Descriptions:
Modify the Claude prompt to focus on specific aspects important to you

### Add Custom Fields:
- Add sentiment analysis
- Extract key quotes
- Count engagement metrics
- Identify discussion threads

## Troubleshooting

### Error: "Tweet not found"
- The tweet might be deleted or private
- Check if the URL format is correct
- Try the alternative tweet fetching methods

### Error: "Notion database not found"
- Ensure the integration has access to your database
- Check that database ID is correct

### Categorization quality issues:
- Increase Claude's max tokens for more detailed analysis
- Adjust temperature (lower = more consistent, higher = more creative)
- Refine the prompt with specific examples

### Rate limiting:
- vxtwitter: Rarely rate limited
- Claude API: Haiku has generous limits
- Adjust polling frequency if hitting limits

## Cost Estimate

**Claude Haiku 4.5 pricing** (as of Dec 2024):
- Input: $0.80 per million tokens
- Output: $4.00 per million tokens

**Estimated cost per tweet**: ~$0.001-0.002
- 100 tweets/day = ~$0.10-0.20/day
- 3,000 tweets/month = ~$3-6/month

Very affordable for this use case!

## Advanced: Batch Processing Existing Links

If you have many existing unprocessed links:

1. Select all items in Notion
2. Set "Processing Status" to "Pending" for all
3. The workflow will process them automatically
4. Monitor n8n execution log for any errors

## Next Steps & Enhancements

Once this is working, you can:
- Add similar workflows for other content types
- Create Notion views filtered by category/tags
- Set up Telegram/Slack notifications for interesting finds
- Build a "best of" automated newsletter from top categorized tweets
- Export to other tools (Obsidian, Roam, etc.)

## Support

If you encounter issues:
1. Check n8n execution logs for errors
2. Test each node individually
3. Verify all credentials are valid
4. Check Notion integration permissions

---

**Pro tip**: Start with 5-10 test tweets to verify everything works before processing your full collection!
