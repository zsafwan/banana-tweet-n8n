# Prompt Extraction - How It Works

## Overview
The workflow uses Claude AI to intelligently identify and extract nano banana pro prompts from tweets. Here's what to expect:

## What Gets Extracted

### ✅ WILL Extract:

**Example 1: Quoted Prompts**
```
Tweet: "Here's a killer nano banana prompt: 'analyze this code for bugs and suggest improvements' - works every time!"

Extracted Prompt: analyze this code for bugs and suggest improvements
```

**Example 2: Code Block Prompts**
```
Tweet: Try this nano banana prompt:
```
Generate a weekly meal plan for a vegetarian family of 4, 
budget $200, include shopping list
```

Extracted Prompt: Generate a weekly meal plan for a vegetarian family of 4, budget $200, include shopping list
```

**Example 3: Clear Format**
```
Tweet: 🔥 Nano banana prompt that changed my workflow:

"Create a project timeline with milestones, risks, and resource allocation for [project description]"

Game changer! 🚀

Extracted Prompt: Create a project timeline with milestones, risks, and resource allocation for [project description]
```

**Example 4: Multi-line Structured Prompts**
```
Tweet: My go-to nano banana prompt structure:

Role: Expert financial analyst
Task: Analyze Q3 earnings report
Output: Executive summary + 5 key insights
Format: Bullet points

Extracted Prompt: Role: Expert financial analyst
Task: Analyze Q3 earnings report
Output: Executive summary + 5 key insights
Format: Bullet points
```

**Example 5: Embedded in Thread**
```
Tweet: After 100+ experiments, this nano banana prompt is 🔥

"You are a senior developer reviewing code. Identify: 1) Security issues 2) Performance bottlenecks 3) Best practice violations. Be specific."

Works on ANY codebase.

Extracted Prompt: You are a senior developer reviewing code. Identify: 1) Security issues 2) Performance bottlenecks 3) Best practice violations. Be specific.
```

### ❌ WON'T Extract (Empty String):

**Example 1: Discussion Without Actual Prompt**
```
Tweet: "Nano banana prompts are amazing for code review! I've been using them all week and my productivity is up 50%."

Extracted Prompt: [empty]
Reason: Discusses prompts but doesn't contain one
```

**Example 2: Vague Reference**
```
Tweet: "Try using nano banana prompts for your daily standup notes - they work great!"

Extracted Prompt: [empty]
Reason: Suggests usage but no actual prompt
```

**Example 3: Partial/Incomplete**
```
Tweet: "Start your nano banana prompt with 'you are an expert' and then..."

Extracted Prompt: [empty]
Reason: Fragment/instruction about prompts, not a complete prompt
```

**Example 4: Question About Prompts**
```
Tweet: "What are your favorite nano banana prompts for content creation? Need ideas!"

Extracted Prompt: [empty]
Reason: Asking for prompts, not sharing one
```

## Edge Cases Handled

### Multiple Prompts in One Tweet
```
Tweet: "Two nano banana prompts I use daily:

1. 'Summarize this article in 3 bullets'
2. 'Generate 5 headline alternatives'

Both are 🔥"

Extracted Prompt: Summarize this article in 3 bullets
Note: Extracts the primary/first prompt
```

### Prompt with Examples
```
Tweet: Nano banana prompt template:

"Generate [number] [content_type] about [topic] for [audience]"

Example: "Generate 5 blog post ideas about AI automation for small business owners"

Extracted Prompt: Generate [number] [content_type] about [topic] for [audience]
Note: Extracts the template, not the example
```

### Prompt with Hashtags/Emojis
```
Tweet: 🚀 #nanobananapro prompt: 

"Act as a UX researcher. Review this interface and suggest 3 improvements with reasoning." 

#AI #productivity

Extracted Prompt: Act as a UX researcher. Review this interface and suggest 3 improvements with reasoning.
Note: Cleans up hashtags and emojis
```

## How Claude Decides

Claude uses these heuristics:

1. **Quoted text** → High probability it's the prompt
2. **Code blocks** → Very high probability
3. **"Here's the prompt:"** → Extract what follows
4. **Clear structure** (Role/Task/Output) → Extract entire structure
5. **Instructional language** → Check if it's a complete instruction
6. **Context clues** → "I use this prompt", "try this prompt"

## Quality Indicators

Claude will extract prompts that have:
- ✅ Clear instructions or commands
- ✅ Specific tasks or outcomes
- ✅ Complete thoughts (not fragments)
- ✅ Actionable content

Claude will NOT extract:
- ❌ Questions about prompts
- ❌ Meta-discussions about prompting
- ❌ Incomplete fragments
- ❌ Generic advice without specific prompts

## Notion Database Usage Tips

### Search for Prompts:
1. **Filter by Category**: "Examples" category will have the most actual prompts
2. **Check Prompt field**: Non-empty = contains extracted prompt
3. **Empty Prompt field**: The tweet discusses prompts but doesn't contain one

### Create Views:
- **"Prompts Only"**: Filter where Prompt field is NOT empty
- **"By Use Case"**: Group by Category + filter non-empty Prompts
- **"Needs Review"**: Tag = "needs-review" (extraction uncertain)

### Best Practices:
- Review first 10-20 extractions to verify quality
- Tag false positives for training/improvement
- Create a "Verified" tag for manually confirmed prompts
- Use the Prompt field for quick copy-paste

## Improving Extraction Accuracy

If you notice patterns that aren't being extracted well:

1. **Adjust the Claude prompt** in the workflow to emphasize those patterns
2. **Add specific examples** to the prompt about what to extract
3. **Increase max_tokens** if prompts are being truncated
4. **Review "Discussion" category** items - may contain missed prompts

## Example Notion Queries

**Find all actionable prompts:**
```
Filter: Prompt is not empty AND Category is Examples
```

**Find prompts for specific use case:**
```
Filter: Prompt is not empty AND Tags contains "code-review"
```

**Find prompts that need verification:**
```
Filter: Prompt is not empty AND Processing Status is Processed
Sort: Date Processed (newest first)
Limit: 20
```

---

**Remember**: The AI is good but not perfect. A quick manual review of extractions will help you identify any patterns that need adjustment in the workflow prompt!
