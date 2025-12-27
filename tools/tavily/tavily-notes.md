# Tavily Web Search MCP

Personal Quick reference site for Tavily usage



## Tavily Crash Course Notes

Created on 12/27/2025



### 1. Why agents need web search?

- LLMs have inherent limits:
  - trained on static data. Unless new trainings are performed, LLMs have no native way to learn new events. 
  - Internal reasoning alone causes **hallucinations**. 
  - **limited context window** to hold long content. E.g., long articles, full documentation sets. 
- Web search allows LLMs to:
  - validate information with **multiple independent sources**. 
  - run **multi-step searching** which allows iterative reasoning. 
  - access niche or domain-specific data that wasn’t in the training data. 



### 2. Keyword, semantic, and hybrid search

> Understanding the three main types of search: keyword, semantic, and hybrid - helps you **design smarter queries, reduce irrelevant results, and improve the accuracy** of your agent's reasoning. 

#### 2.1 Keyword search 

- Must match literal tokens
- Ranking relies on frequency, titles, metadata etc. 

- Misses content that uses different phrasing

- Agents relying only on keyword search often retrieve narrow, incomplete, or noisy results.

#### 2.2 Semantic search

- Uses **embeddings** to represent meaning. (**an embedding is a vector** that represents a token aka a piece of text)
- Measures similarity between query vector and document vectors.
- Semantic search is the foundation of modern agent search because agents often ask open-ended, contextual questions.

#### 2.3 Hybrid search

- Uses semantic matching to understand intent.
- Uses keyword relevance for factual groundings.
- Ranks results using a **weighted combination of both signals.**
- Limits: more **expensive**, may need manual tuning. 

#### 2.4 Example Scenarios

| SCENARIO                                                   | BEST SEARCH TYPE   | REASON                                  |
| :--------------------------------------------------------- | ------------------ | --------------------------------------- |
| You need high precision for very specific terms            | Keyword            | Exact matching                          |
| You want conceptually relevant results for broad questions | Semantic           | Understands meaning                     |
| You need both precision and strong intent matching         | Hybrid             | Balanced approach                       |
| Research tasks needing deep exploration                    | Hybrid or Semantic | Captures broader context                |
| Technical or documentation-based questions                 | Keyword + Semantic | Finds exact API names plus explanations |



### 3. Search Quality Concepts

#### 3.1 Key parameters

- **Recall** measures how many relevant documents are retrieved, even if some irrelevant ones appear too.
- **Precision** measures how relevant the retrieved results are, even if the agent misses a few items.

> Agents often **begin with high recall** (broad search) and **refine toward high precision** (filtered search).

- **Recency** determines how up-to-date your information is.
- **Domain filters** let agents control which sources they use, while domain trust scoring evaluates how credible each site is. 
- **Depth** determines how far a search engine explores past the initial set of results.

#### 3.2 Some failure modes

**Too broad query.** Fix: Add filters, specify topic, increase precision.

**Too narrow query.** Fix: Expand the query, generalize, or increase depth.

**Outdated Sources.** Fix: Apply recency filters or validate timestamps.

**Low-Quality Domains.** Fix: Exclude domains or prioritize trustworthy sources.



### 4. Using Tavily Search

#### 4.1 Tavily Search Basics

Using Tavily Search **as-is** gives you:

• Clean, structured results that are easy for LLMs to consume

• Lower chance of junk data from scraping or messy HTML

• A simple, consistent integration path - agents built on it are easier to maintain

• Real-time access to up-to-date web content, improving answer relevance and freshness

```shell
pip install tavily-python
```

```python
from tavily import TavilyClient

tavily_client = TavilyClient(api_key="tvly-YOUR_API_KEY")

response = tavily_client.search("Who is Leo Messi?")

print(response)
```



#### 4.2 Search Parameters

| PARAMETER                           | WHAT IT CONTROLS                                             | DEFAULT / NOTES                                              |
| ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `query` (string)                    | The search keywords or natural language query                | Required                                                     |
| `search_depth` (str)                | The depth of the search (how focused and thorough results are) | "basic" returns generic snippets; "advanced" returns more tailored, highly relevant content (higher latency, default: "basic") |
| `topic` (str)                       | The search domain type - influences which search agent is used (e.g. general, news, finance) | "general" by default.                                        |
| `time_range` (str / int)            | Time-filter for when source content was published - useful for freshness | Values like "day", "week", "month", "year" or short forms "d", "w", "m", "y". |
| `max_results` (int)                 | How many results to return (i.e. cap on number of urls)      | Default: 5 unless overridden.                                |
| `include_raw_content`(bool)         | Whether to return the full cleaned HTML/text content of each result (not just snippet) | Default false. "markdown" or true to return results content in markdown. "text" to return in plain text. |
| `include_images` (bool)             | If true, includes images related to the query in response    | Default False. Useful for multimedia or image-based agent tasks. |
| `include_answer` (bool or str)      | Option to ask Tavily to generate a short answer (via LLM) based on search results | Default False. If "basic" or true returns a quick answer. "advanced" returns a more detailed answer. |
| `include_domains` (list of strings) | Whitelist of domains. Only include results from these domains | Default None. Useful when you want authoritative sources only. |
| `exclude_domains` (list of strings) | Blacklist of domains. Explicitly omit results from these domains | Default None. Helps filter out low-quality or irrelevant content. |

Example Usage

```python
from tavily import TavilyClient

tavily_client = TavilyClient(api_key="tvly-YOUR_API_KEY")

response = tavily_client.search(
    query="latest trends in generative AI 2025",
    search_depth="advanced",
    topic="news",
    time_range="w",        # past week
    max_results=10,
    include_raw_content=True,
    include_images=False,
    include_answer=False,
    exclude_domains=["some-spammy-site.com","low-quality-blog.org"]
)

for res in response["results"]:
    print(res["title"], res["url"])
    print(res["content"][:200], "...")
```

Tavily returns: 

```json
How 2025 Recalibrated AI Models Race - Forbes https://www.forbes.com/sites/geruiwang/2025/12/26/how-2025-recalibrated-ai-models-race/
Another important trend in 2025 was sustained effort to reduce hallucinations. Developers increasingly treat hallucination as a systems problem.

Several techniques gained traction. Reinforcement-lear ...
What the tech happened in 2025? - The Drum http://www.thedrum.com/opinion/what-the-tech-happened-in-2025
The industry was quick to pick holes in generative AI’s output. After a short-lived excitement of its capabilities, everyone jumped on the bandwagon of “look at the seven fingers” or “it can’t even ha ...
9 Brands That Doubled Down On AI in 2025 - ADWEEK https://www.adweek.com/brand-marketing/9-brands-that-doubled-down-on-ai-in-2025/
Subscribe Sign In

 Agencies
 Brands
 Creativity
 Media
 TV
 Commerce
 Tech
 Creators

### Year in Review

# 9 Brands That Doubled Down On AI in 2025

## Coca-Cola, Disney, Duolingo, and more embraced ...
#6 most read from article: Generative AI for Manufacturing 4.0 - Today's Medical Developments https://www.todaysmedicaldevelopments.com/news/most-read-generative-artificial-intelligence-for-manufacturing-40/
Inventory and supply chain management: Generative AI can improve inventory tracking and demand forecasting by analyzing historical and real-time data. IBM’s research shows that generative AI-driven to ...
As More Coders Adopt AI Agents, Security Pitfalls Lurk in 2026 - Dark Reading https://www.darkreading.com/application-security/coders-adopt-ai-agents-security-pitfalls-lurk-2026
Already, developers have fully integrated AI-code generation and analysis into their workflow. An October 2025 survey conducted by development-tool maker JetBrains found that 85% of the nearly 25,000  ...
AI Image Generators Default to the Same 12 Photo Styles, Study Finds - Gizmodo https://gizmodo.com/ai-image-generators-default-to-the-same-12-photo-styles-study-finds-2000702012
Subscribe and interact with our community, get up to date with our customised Newsletters and much more.

## Related Articles

### The Best Tech Gifts of 2025

Gadgets gifts are the best gifts to get  ...
AI supercharges scientific output while quality slips - ScienceDaily http://www.sciencedaily.com/releases/2025/12/251224032347.htm
Aug. 11, 2023  From text-generating ChatGPT to voice-activated Siri, artificial intelligence-powered tools are designed to aid our everyday life -- as long as you speak a language they support. These ...
AI Image Generators Resort Back to the Same 12 Photo Styles, Study Calls It 'Visual Elevator Music' - PetaPixel https://petapixel.com/2025/12/23/ai-image-generators-resort-back-to-the-same-12-photo-styles-study-calls-it-visual-elevator-music/
In a paper published in the journal Patterns00299-5), a research team tested two AI models: Stable Diffusion XL, an image generator, and LLaVA, an image description model. Gizmodo reports that they us ...
The AI Mandate for 2026: Building an intelligent workforce - Fortune India https://www.fortuneindia.com/amp/story/opinion/the-ai-mandate-for-2026-building-an-intelligent-workforce-2/129019
The ‘Wild West’ of agents ends; the era of fixed workflows begins | Credits: Getty Images

Set Fortune India ✓As Preferred on Google

In 2025, enterprises focussed on driving ROI with their AI investm ...
What 2025 Taught Us About The Future Of Construction Tech - Forbes https://www.forbes.com/sites/angelicakrystledonati/2025/12/23/what-2025-taught-us-about-the-future-of-construction-tech/
, and you acknowledge our Privacy Statement.

Sign Up Now No Thanks

You're Subscribed!

Finish creating a Forbes account to manage your contact settings and preferences.

Create An Account

Explore M ...
```



#### 4.3 Best Practices

- **Use defaults for quick fact-lookups and basic queries**
  - E.g., low max_results, default search depth, now raw content extraction. 
  - Benefits: low latency, small results

- **Raise depth and results for complex tasks**
  - For research, summarization, or RAG (retrieval-augmented generation) tasks needing deep context: increase search_depth, raise max_results, possibly enable include_raw_content.
- **Targeted relevance - domain filters and time filters**
  - Use include_domains (whitelist) to restrict results to known good domains.
  - Use exclude_domains to avoid low-quality or untrusted content.
  - Apply recency filters (e.g. topic = "news", time_range/days) to prioritize fresh content.
- **Cache and Reuse Results When Possible**
- **Combine multiple Tavily tools**
  - Use **search** for discovery of relevant links/pages.
  - For deeper content (full article, structured extraction), follow up with **extract, crawl, or map** where appropriate. This avoids overloading search results while giving richer content only when needed.
  - For RAG pipelines: search → extract → summarize/ingest into vector store. This pattern balances cost, depth, and performance.
- **Account for Latency and Rate Limits**
  - Complex queries (advanced search) may take longer.
- **Use Structured Output - Avoid Parsing Raw HTML in Agent Logic**
  - Tavily returns clean, structured **JSON** optimized for LLMs and agents



#### 4.4 Sample Parameter Strategy by Use Case

| USE CASE                              | RECOMMENDED CONFIG / STRATEGY                                |
| ------------------------------------- | ------------------------------------------------------------ |
| Quick fact lookup / Q&A               | Default search settings, max_results = 5, no raw content, basic depth |
| Up-to-date news / recent events       | Use topic = "news", set time_range to "day" or "week", limit results, optionally include raw content for summarization |
| Research / summarization / RAG        | search_depth = advanced, max_results = 10–20, enable include_raw_content, caching + follow-up extraction |
| Trusted-source retrieval (docs, spec) | Use include_domains whitelist, maybe combine with domain-specific crawls or extract calls |
| High-volume or batch queries          | Cache results, rate-limit, batch queries, reuse previous outputs where possible |



#### 4.5 Example: Optimized Search Call for a Research Agent

```python
from tavily import TavilyClient

tavily_client = TavilyClient(api_key="YOUR_API_KEY")

response = tavily_client.search(
    query="2025 trends in web-agent search APIs",
    search_depth="advanced",
    max_results=10,
    include_raw_content=True,
    include_images=False,
    include_answer=False,
    exclude_domains=["spammy-site.com","low-quality-blog.org"]
)
```



### 5. Using Tavily Extract

[Tavily Extract API Documentation](https://docs.tavily.com/documentation/api-reference/endpoint/extract)

Use **Extract** when:

- You need more than just brief snippets (e.g. full articles, documentation pages, long-form content).

- Your agent needs entire page context, for summarization, quoting, data extraction, or deeper analysis.

- You want to avoid brittle HTML parsing yourself, Tavily handles cleaning and structuring.

> There is a practical limit of up to 20 URLs per Extract request, requests with more than 20 URLs will return a 400 error.



#### 5.1 Example Request

```python
from tavily import TavilyClient

client = TavilyClient(api_key="tvly-YOUR_API_KEY")

response = client.extract(
    urls=["https://en.wikipedia.org/wiki/Artificial_intelligence"],
    extract_depth="basic"  # or "advanced"
)

for res in response["results"]:
    print("URL:", res["url"])
    print("Content (start):", res["raw_content"][:300])
```

Expected response structure: 

```json
{
  "results": [
    {
      "url": "https://en.wikipedia.org/wiki/Artificial_intelligence",
      "raw_content": "Artificial intelligence (AI) is intelligence demonstrated by machines...",
      "images": [],
      // possibly other metadata fields
    }
  ],
  "failed_results": [],
  "response_time": 0.8
}
```



#### 5.2 Compare with **Search**

| USE-CASE OR REQUIREMENT                                      | USE TAVILY SEARCH | USE TAVILY EXTRACT            |
| ------------------------------------------------------------ | ----------------- | ----------------------------- |
| Quick fact lookup or short answers                           | ✅                 | ❌ (Overkill)                  |
| Getting titles, URLs, snippets for many pages                | ✅                 | ❌                             |
| Need full article/document content for summarization or indexing | ❌ (Snippets only) | ✅                             |
| Building a knowledge base, ingestion pipeline, or long-form summarization | ❌ (Incomplete)    | ✅                             |
| Low latency, minimal token usage tasks                       | ✅                 | ❌ (Potentially large content) |

#### 5.3 Extract Best Practices

- Use a **two-step process for reliability**: first use Search to find relevant URLs, then use Extract only on top results, this reduces noise and avoids extracting irrelevant pages.
- For complex or dynamic web pages (with embedded media, structured data, tables), use extract_depth = "advanced" for better coverage.
- **Avoid bulk extracting large numbers of URLs**, respect the limit (≤ 20 URLs per request). If you need more, split into batches.
- **Combine Extract with domain filters, recency filters, or other heuristics** (from search) to prioritize high-quality, relevant sources before extraction.



### 6. Using Tavily Crawl and Map

[Tavily Map API Documentation](https://docs.tavily.com/documentation/api-reference/endpoint/map)

- Tavily **Crawl** is a graph-based website traversal tool that **lets your agent explore many links in parallel across a website**, following hyperlinks, discovering pages, and fetching their content automatically.

- **Crawl** is useful when you need to **scrape or index multiple pages under a domain**: documentation sites, blogs, archives, product catalogs, or any site with many **nested or paginated sections**.

[Tavily Crawl API Documentation](https://docs.tavily.com/documentation/api-reference/endpoint/crawl)

- **Map** is Tavily's **structure-first tool**: it walks a site like a graph and returns a list of URLs (a sitemap) starting from a root URL, without extracting full content. Use it when you want to **understand what's on a site** before paying to ingest everything.

  - Great for building URL **indexes** for docs sites, blogs, or wikis.

  - Helps reveal navigation structure and page hierarchy.

  - Often **used as a first step before selective Extract or focused Crawl**.



#### 6.1 Craw Parameters

| PARAMETER                                      | WHAT IT CONTROLS                                             |
| ---------------------------------------------- | ------------------------------------------------------------ |
| `url` (string)                                 | The base URL from where the crawl begins (root domain or subdomain) |
| `max_depth` (int)                              | How many link-hops away from the root the crawl can reach (e.g. 1 = only pages directly linked, 2 = pages linked from those pages, etc.) |
| `max_breadth` (int)                            | How many links per page the crawler will follow (i.e. how wide each level of traversal is) |
| `limit` (int)                                  | Total number of links/pages the crawl will process before stopping, useful to cap cost and runtime |
| `select_paths` / `select_domains` (string[])   | Regex filters to include only URLs matching certain path patterns or domains (e.g. /blog/.*, only docs subdomain) |
| `exclude_paths` / `exclude_domains` (string[]) | Regex filters to exclude certain paths/domains (e.g. /private/.*, admin pages) |
| `allow_external` (bool)                        | Whether links to external domains are allowed (if you want to stay within the same domain or permit external links) |
| `extract_depth` (enum: "basic" or "advanced")  | Controls how the crawl extracts content from each page: basic may fetch main text; advanced tries to include embedded content, tables, etc. |
| `include_images` (bool)                        | Optionally include image data from pages, useful when extracting multimedia or rich content |
| `instructions` (string)                        | Natural language instructions for the crawler.               |



#### 6.2 Example Crawl Request

Use-case example: Crawl the docs site to gather all documentation pages (SDK docs, API reference, tutorials), then extract full clean content to build a searchable knowledge base or vector store.

```python
from tavily import TavilyClient

tavily_client = TavilyClient(api_key="tvly-YOUR_API_KEY")

response = tavily_client.crawl(
    url="https://docs.tavily.com",
    max_depth=3,
    max_breadth=30,
    limit=100,
    select_paths=["/documentation/.*", "/sdk/.*"],
    exclude_paths=["/private/.*", "/admin/.*"],
    allow_external=False,
    extract_depth="advanced",
    include_images=False
)

for page in response["results"]:
    print(page["url"])
    print(page["raw_content"][:200], "...")
```

- This is exactly how a "Crawl → RAG" pipeline works: crawl, extract, embed, and query later for retrieval-augmented generation.



#### 6.3 Crawl and Map Best Practices

- Because Crawl may visit many pages, it's recommended to apply filters (select_paths, select_domains, etc.) to avoid irrelevant sections and limit noise.
-  If you only need page URLs (not full content), consider using Map instead, faster and cheaper.





