---
name: search-software-context
description: Search software platform documentation using Ferris EMU. Use when implementing integrations, troubleshooting platform-specific issues, looking up API references, or answering questions about software tools the team uses.
---

# Search Software Context

Use the `search_software_context` tool to find relevant documentation about software platforms. Results come from curated documentation indexed by Ferris using semantic memory (Hindsight), not generic web search.

## When to Use

- Implementing or debugging integrations with a specific platform (e.g. Qualtrics, EngineHire, Salesforce)
- Looking up API endpoints, configuration options, or SDK usage
- Understanding platform concepts, workflows, or data models
- Answering "how do I do X in platform Y?" questions
- Troubleshooting platform-specific errors or behaviors

## Tool Parameters

| Parameter | Type    | Required | Description                                                                |
| --------- | ------- | -------- | -------------------------------------------------------------------------- |
| `query`   | string  | Yes      | Natural language search query                                              |
| `bank`    | string  | Yes      | Which platform to search. Dynamic enum — the server populates valid values at runtime based on which banks are active for your project. Current options include `qualtrics`, `enginehire`, and `salesforce-developer`. |
| `limit`   | integer | No       | Max results to return (default: 10)                                        |

## Writing Effective Queries

The search engine uses four complementary retrieval strategies simultaneously. Understanding them helps you write queries that get the best results.

### 1. Semantic Search (meaning-based)

Understands the intent behind your words. You don't need to match exact terminology.

**Good queries:**

- "How to distribute surveys via SMS" (finds Twilio/SMS distribution docs even if they use different words)
- "Setting up patient feedback collection" (finds CSAT/NPS survey configuration)
- "Authentication flow for API access" (finds OAuth/token docs regardless of exact naming)

### 2. Keyword Search (exact term matching)

Finds results containing specific names, identifiers, and technical terms. Include proper nouns and exact API names.

**Good queries:**

- "XM Directory contactId lookup" (exact field name triggers keyword match)
- "REST API v3 POST /surveys endpoint" (specific version + path)
- "HIPAA compliance data residency requirements"

### 3. Graph Traversal (relationship-based)

Follows connections between concepts. Mention entities when you need related information.

**Good queries:**

- "What integrations does the survey distribution system connect to?" (follows entity links)
- "How does the dashboard relate to survey responses?" (traverses concept relationships)
- "What components depend on the contact directory?"

### 4. Temporal Search (time-aware)

Filters by when documentation was created or updated. Mention timeframes when relevant.

**Good queries:**

- "API changes in 2024" (scopes to time window)
- "Recently added survey question types"
- "Deprecated endpoints"

## Query Formulation Tips

### Be specific, not vague

```
Bad:  "how does it work"
Good: "How does Qualtrics survey distribution trigger emails after form submission"
```

### Include the platform name for focused results

```
Bad:  "API authentication"
Good: "Qualtrics API OAuth2 authentication setup"
```

### Ask complete questions

```
Bad:  "webhooks"
Good: "How to configure webhooks for survey completion events in Qualtrics"
```

### Combine terms from different retrieval strategies

The best queries activate multiple strategies simultaneously:

```
"How to configure Qualtrics XM Directory contact imports via REST API"
 ^-- semantic meaning    ^-- exact keyword         ^-- keyword match
```

### Always specify `bank`

The `bank` parameter is required. It is a dynamic enum — the server populates the valid values at runtime based on which banks are active for your project. Current options include `qualtrics`, `enginehire`, and `salesforce-developer`. If you pass an invalid bank, the error response lists the available options.

### Iterate on partial results

If the first query returns partial information:

1. Note which platform/bank had the best results
2. Re-query with `bank` set to that platform
3. Rephrase to be more specific based on terminology you saw in results

## Interpreting Results

Results include:

- **Text content**: The relevant documentation passage
- **Score**: Relevance ranking (higher = more relevant)
- **Source bank**: Which platform the result came from

Results are sorted by relevance across all strategies. A result appearing via multiple strategies (e.g. both semantically similar AND keyword-matched) ranks higher than one matched by a single strategy.

## Examples

### Looking up a specific API

```json
{
  "query": "How to create a survey distribution using the Qualtrics API",
  "bank": "qualtrics",
  "limit": 5
}
```

### Searching EngineHire docs

```json
{
  "query": "webhook configuration for event-driven notifications",
  "bank": "enginehire",
  "limit": 10
}
```

### Searching Salesforce developer docs

```json
{
  "query": "Apex REST API callout authentication with named credentials",
  "bank": "salesforce-developer",
  "limit": 5
}
```

### Debugging an integration issue

```json
{
  "query": "401 unauthorized error when calling distribution API with OAuth token",
  "bank": "qualtrics",
  "limit": 5
}
```
