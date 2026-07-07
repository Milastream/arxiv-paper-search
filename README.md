[Arxiv Paper Search](https://apify.com/ryanclinton/arxiv-paper-search?fpr=data)

Search and extract preprint research papers from the ArXiv open-access repository. Query over 2.4 million academic papers across physics, mathematics, computer science, biology, economics, and more with structured JSON output, no API key required.

## What does ArXiv Preprint Paper Search do?

ArXiv Preprint Paper Search is an Apify actor that queries the [ArXiv API](https://info.arxiv.org/help/api/index.html) to find and extract scholarly preprint papers based on keywords, authors, abstracts, and subject categories. ArXiv is the world's largest open-access preprint server, hosting research papers across disciplines including computer science (cs.AI, cs.CL, cs.LG), mathematics, physics, quantitative biology, quantitative finance, statistics, and electrical engineering.

This actor returns structured JSON data for each matching paper, including the full title, abstract, author list, publication and update dates, ArXiv category classifications, direct PDF download links, DOI references, and journal citation information. It handles ArXiv API rate limits automatically and supports paginated retrieval of up to 5,000 papers per run.

Whether you are conducting a literature review, tracking research trends in machine learning and artificial intelligence, monitoring new publications from specific authors, or building a research database, this actor provides a reliable pipeline from ArXiv to structured data you can integrate into any workflow.

## Why use ArXiv Preprint Paper Search on Apify?

Running this actor on the Apify platform gives you several advantages over calling the ArXiv API directly:

- **No infrastructure to manage.** The actor runs in the cloud, handles rate limiting (ArXiv requires a minimum 3-second delay between requests), and automatically paginates through large result sets.
- **Structured output.** Raw ArXiv Atom XML feeds are parsed and transformed into clean JSON records ready for analysis, database import, or integration with other tools.
- **Scheduling and automation.** Set up recurring runs to monitor new papers in your research area on a daily or weekly basis using Apify's built-in scheduler.
- **Integration-ready.** Push results directly to Google Sheets, webhooks, Slack, email, or any downstream system using Apify integrations.
- **No API key required.** ArXiv's API is open and free. This actor wraps it with proper rate-limit compliance so you never get blocked.

## Key features

- **Full-text search** across titles, abstracts, authors, and comments using ArXiv's query syntax
- **Category filtering** by any ArXiv subject classification (cs.AI, cs.CL, math.CO, stat.ML, physics.hep-th, and hundreds more)
- **Advanced query syntax** with field prefixes (`ti:`, `au:`, `abs:`, `cat:`, `co:`, `all:`) and Boolean operators (`AND`, `OR`, `ANDNOT`)
- **Flexible sorting** by relevance, submission date, or last updated date in ascending or descending order
- **Paginated retrieval** of up to 5,000 papers per run with automatic rate-limit handling
- **Complete metadata extraction** including ArXiv ID, title, abstract, authors, categories, PDF URL, abstract URL, DOI, journal reference, and author comments
- **Clean JSON output** with whitespace-normalized text fields and properly formatted ISO 8601 date strings

## How to use ArXiv Preprint Paper Search

1. Navigate to the [actor's input page](https://apify.com/ryanclinton/arxiv-paper-search) on Apify.
2. Enter a **Search Query** using plain keywords or ArXiv field prefixes. For example:

- `all:large language models` -- search across all fields
- `ti:transformer AND au:vaswani` -- papers with "transformer" in the title by author Vaswani
- `abs:reinforcement learning` -- search within abstracts only
3. Optionally specify a **Category** to narrow results to a specific ArXiv subject (e.g., `cs.AI` for Artificial Intelligence, `cs.CL` for Computation and Language).
4. Choose your preferred **Sort By** method (Relevance, Last Updated, or Submission Date) and **Sort Order** (Descending or Ascending).
5. Set **Max Results** to control how many papers to return (1 to 5,000).
6. Click **Start** and wait for the run to complete. Results appear in the dataset tab.

You can also call this actor programmatically via the Apify API or integrate it into larger automation workflows.

## Input parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `searchQuery` | String | No* | - | Search query with optional field prefixes: `all:` (default), `ti:` (title), `au:` (author), `abs:` (abstract), `cat:` (category), `co:` (comment). Combine with `AND`, `OR`, `ANDNOT`. |
| `category` | String | No* | - | ArXiv category filter such as `cs.AI`, `cs.CL`, `math.CO`, `physics.hep-th`, or `stat.ML`. See the full taxonomy at [arxiv.org/category_taxonomy](https://arxiv.org/category_taxonomy). |
| `sortBy` | Select | No | `relevance` | How to sort results. Options: `relevance`, `lastUpdatedDate`, `submittedDate`. |
| `sortOrder` | Select | No | `descending` | Sort direction. Options: `descending`, `ascending`. |
| `maxResults` | Integer | No | `50` | Maximum number of papers to return. Range: 1 to 5,000. |

*At least one of `searchQuery` or `category` must be provided.

### Input examples

**Basic keyword search:**

```
{
    "searchQuery": "all:large language models",
    "maxResults": 100
}
```

**Category filter for AI papers:**

```
{
    "category": "cs.AI",
    "sortBy": "submittedDate",
    "sortOrder": "descending",
    "maxResults": 200
}
```

**Author search combined with title filter:**

```
{
    "searchQuery": "au:hinton AND ti:deep learning",
    "sortBy": "submittedDate",
    "maxResults": 50
}
```

**Recent machine learning papers across categories:**

```
{
    "searchQuery": "abs:diffusion model",
    "category": "cs.LG",
    "sortBy": "lastUpdatedDate",
    "sortOrder": "descending",
    "maxResults": 500
}
```

### Tips for best results

- **Use field prefixes for precision.** Instead of a broad search like `neural networks`, try `ti:neural networks` to match only titles, or `au:hinton AND ti:deep learning` to find specific author-topic combinations.
- **Combine search query with category.** Setting both `searchQuery` and `category` applies an AND filter, which significantly narrows results. For example, searching `transformer` with category `cs.CL` returns only computational linguistics papers about transformers.
- **Sort by submission date for recent papers.** If you want the newest research, set `sortBy` to `submittedDate` and `sortOrder` to `descending`.
- **Boolean operators must be uppercase.** Use `AND`, `OR`, and `ANDNOT` (not lowercase). Example: `ti:attention AND abs:transformer ANDNOT au:vaswani`.
- **Quote multi-word terms for exact matching.** ArXiv's API treats spaces within a field prefix as AND by default. Use `ti:"attention is all you need"` for exact phrase matching in titles.
- **Monitor research trends.** Schedule this actor to run weekly with a category filter to track new papers in your field automatically.

## Programmatic access

You can call ArXiv Preprint Paper Search programmatically from any language or tool that supports HTTP requests. Below are examples using the Apify API with the actor ID `ryanclinton/arxiv-paper-search`.

**Python:**

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")

run = client.actor("ryanclinton/arxiv-paper-search").call(run_input={
    "searchQuery": "all:large language models",
    "category": "cs.CL",
    "sortBy": "submittedDate",
    "sortOrder": "descending",
    "maxResults": 100,
})

for paper in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{paper['arxivId']} - {paper['title']}")
    print(f"  PDF: {paper['pdfUrl']}")
```

**JavaScript:**

```
import { ApifyClient } from "apify-client";

const client = new ApifyClient({ token: "YOUR_API_TOKEN" });

const run = await client.actor("ryanclinton/arxiv-paper-search").call({
    searchQuery: "all:large language models",
    category: "cs.CL",
    sortBy: "submittedDate",
    sortOrder: "descending",
    maxResults: 100,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((paper) => {
    console.log(`${paper.arxivId} - ${paper.title}`);
    console.log(`  PDF: ${paper.pdfUrl}`);
});
```

**cURL:**

```
curl "https://api.apify.com/v2/acts/ryanclinton~arxiv-paper-search/run-sync-get-dataset-items?token=YOUR_API_TOKEN" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "searchQuery": "all:large language models",
    "category": "cs.CL",
    "sortBy": "submittedDate",
    "maxResults": 50
  }'
```

## Output example

Each paper in the output dataset contains the following fields:

```
{
    "arxivId": "2401.02385v2",
    "title": "Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models",
    "abstract": "Harnessing the power of human-annotated data through Supervised Fine-Tuning (SFT) is pivotal for advancing Large Language Models (LLMs). In this paper, we introduce a novel fine-tuning method called Self-Play fIne-tuNing (SPIN), which starts from a supervised fine-tuned model. At the heart of SPIN lies a self-play mechanism, where the LLM refines its capability by playing against instances of itself...",
    "published": "2024-01-04T18:55:13Z",
    "updated": "2024-01-09T07:43:15Z",
    "primaryCategory": "cs.AI",
    "categories": ["cs.AI", "cs.CL", "cs.LG"],
    "authors": "Zixiang Chen, Yihe Deng, Huizhuo Yuan, Kaixuan Ji, Quanquan Gu",
    "authorList": [
        "Zixiang Chen",
        "Yihe Deng",
        "Huizhuo Yuan",
        "Kaixuan Ji",
        "Quanquan Gu"
    ],
    "pdfUrl": "https://arxiv.org/pdf/2401.02385v2",
    "absUrl": "https://arxiv.org/abs/2401.02385v2",
    "doi": "10.48550/arXiv.2401.02385",
    "journalRef": null,
    "comment": "Published at ICML 2024. Code is available at https://github.com/uclaml/SPIN",
    "extractedAt": "2025-01-15T10:23:45.678Z"
}
```

## Output fields reference

| Field | Type | Description |
| --- | --- | --- |
| `arxivId` | String | ArXiv paper identifier with version suffix (e.g., `2401.02385v2`) |
| `title` | String | Paper title with whitespace normalized |
| `abstract` | String | Full abstract text with whitespace normalized |
| `published` | String | Original submission date in ISO 8601 format |
| `updated` | String | Most recent update date in ISO 8601 format |
| `primaryCategory` | String | Primary ArXiv category classification (e.g., `cs.AI`) |
| `categories` | String[] | All ArXiv categories assigned to the paper |
| `authors` | String | Comma-separated author names as a single string |
| `authorList` | String[] | Array of individual author name strings |
| `pdfUrl` | String or null | Direct URL to the PDF file on ArXiv |
| `absUrl` | String | URL to the paper's abstract page on ArXiv |
| `doi` | String or null | Digital Object Identifier if available |
| `journalRef` | String or null | Journal reference if the paper was published in a journal |
| `comment` | String or null | Author-provided comments (page count, conference, code links) |
| `extractedAt` | String | Timestamp of when the record was extracted, in ISO 8601 format |

## How it works

The actor fetches data from the ArXiv API (`export.arxiv.org/api/query`), which returns results as Atom XML feeds. The pipeline handles XML parsing, pagination, rate limiting, and data transformation to produce clean JSON output.

```
ArXiv Preprint Paper Search Pipeline
 ============================================================================

  Input                  Build Query             Fetch Pages
 +----------------+    +------------------+    +---------------------+
 | searchQuery    | -> | Combine query +  | -> | GET /api/query      |
 | category       |    | cat: prefix with |    | ?search_query=...   |
 | sortBy         |    | AND operator     |    | &start=0            |
 | sortOrder      |    +------------------+    | &max_results=100    |
 | maxResults     |                            | 3.1s delay between  |
 +----------------+                            | paginated requests  |
                                               +---------------------+
                                                         |
                                                         v
  Parse XML                Transform                 Output
 +-------------------+    +------------------+    +------------------+
 | fast-xml-parser   | -> | Extract arXiv ID | -> | Push to Apify    |
 | ignoreAttributes: |    | from URL prefix  |    | dataset as JSON  |
 | false             |    | Find PDF/abs     |    | Log category &   |
 | ensureArray() for |    | links in array   |    | DOI statistics   |
 | XML ambiguity     |    | Normalize text   |    +------------------+
 +-------------------+    +------------------+
```

### XML parsing with fast-xml-parser

ArXiv's API returns data as Atom XML, not JSON. The actor uses [fast-xml-parser](https://www.npmjs.com/package/fast-xml-parser) with `ignoreAttributes: false` and `attributeNamePrefix: '@_'` to preserve XML attributes during parsing. This allows access to critical metadata stored in attributes, such as link types (`@_rel`, `@_type`, `@_title`) and category terms (`@_term`).

### The ensureArray utility for XML ambiguity

A key challenge with XML-to-JSON parsing is that elements appearing once become plain objects, while elements appearing multiple times become arrays. For example, a paper with one author produces `{ name: "Alice" }`, but two authors produce `[{ name: "Alice" }, { name: "Bob" }]`. The `ensureArray()` utility normalizes both forms into arrays, applied to authors, categories, and links on every entry.

### Query field prefix syntax

The ArXiv API supports field-specific search prefixes that can be combined with Boolean operators:

| Prefix | Field | Example |
| --- | --- | --- |
| `all:` | All fields (default) | `all:neural network` |
| `ti:` | Title | `ti:attention mechanism` |
| `au:` | Author | `au:lecun` |
| `abs:` | Abstract | `abs:reinforcement learning` |
| `cat:` | Category | `cat:cs.AI` |
| `co:` | Comment | `co:ICML 2024` |

Combine with `AND`, `OR`, `ANDNOT`: `ti:transformer AND au:vaswani ANDNOT abs:vision`

When a `category` input is provided, the actor appends `AND cat:{category}` to the query automatically.

### ArXiv ID extraction

ArXiv paper identifiers are embedded in full URLs like `http://arxiv.org/abs/2401.02385v2`. The actor strips the URL prefix to produce clean IDs (`2401.02385v2`) suitable for constructing other ArXiv URLs or for use as unique identifiers in databases.

### Link array extraction

Each ArXiv entry contains multiple `<link>` elements with different purposes. The actor identifies them by their attributes:

- **PDF link:** Found by matching `@_title === 'pdf'` or `@_type === 'application/pdf'`
- **Abstract page link:** Found by matching `@_rel === 'alternate'`

If no abstract link is found, the actor constructs one from the ArXiv ID as a fallback.

### Rate limiting

ArXiv requires a minimum 3-second delay between API requests. The actor enforces a 3.1-second delay between paginated fetches to stay compliant. Each page retrieves up to 100 papers, so fetching 500 papers requires approximately 5 requests with a total wait time of about 12-15 seconds.

## How much does it cost to run?

ArXiv Preprint Paper Search is very cost-efficient. The ArXiv API is completely free with no usage fees or API keys. The actor uses minimal compute resources on the Apify platform.

| Scenario | Papers | Approximate time | Estimated cost |
| --- | --- | --- | --- |
| Quick search | 50 | ~5 seconds | ~$0.001 |
| Medium batch | 500 | ~20 seconds | ~$0.005 |
| Large extraction | 5,000 | ~3-4 minutes | ~$0.01-0.02 |

**Note:** Large queries take longer primarily because of the required 3-second delay between paginated API requests, not because of compute intensity. Each page fetches up to 100 papers, so 5,000 papers require approximately 50 pages with ~155 seconds of mandated wait time.

Costs are based on Apify platform usage at 256 MB memory. Actual costs may vary slightly based on network conditions and response sizes.

## Limitations and responsible use

- **Rate limiting adds latency.** The 3-second delay between pages means large result sets (1,000+ papers) take minutes to retrieve. Plan accordingly for time-sensitive workflows.
- **No full-text extraction.** The actor returns abstracts and PDF links, not the full text of papers. Use the `pdfUrl` field to download PDFs separately if full text is needed.
- **Result cap at 5,000.** ArXiv's API and this actor limit results to 5,000 papers per query. For broader coverage, run multiple queries with different category or date filters.
- **ArXiv coverage only.** This actor searches ArXiv preprints specifically. For published journal articles, peer-reviewed papers, or other databases, see the related actors listed below.
- **XML response quirks.** ArXiv's API may return approximate total counts for very broad queries. The `opensearch:totalResults` value is informational and may not exactly match the number of retrievable results.
- **Respect ArXiv's terms of service.** This actor complies with ArXiv's rate limits. Avoid scheduling excessively frequent runs that would place unnecessary load on ArXiv's infrastructure.

### ArXiv category reference

Common ArXiv categories for quick reference:

| Category | Subject | Discipline |
| --- | --- | --- |
| `cs.AI` | Artificial Intelligence | Computer Science |
| `cs.CL` | Computation and Language (NLP) | Computer Science |
| `cs.CV` | Computer Vision and Pattern Recognition | Computer Science |
| `cs.LG` | Machine Learning | Computer Science |
| `cs.CR` | Cryptography and Security | Computer Science |
| `cs.SE` | Software Engineering | Computer Science |
| `cs.RO` | Robotics | Computer Science |
| `cs.DS` | Data Structures and Algorithms | Computer Science |
| `math.CO` | Combinatorics | Mathematics |
| `math.OC` | Optimization and Control | Mathematics |
| `math.ST` | Statistics Theory | Mathematics |
| `stat.ML` | Machine Learning | Statistics |
| `stat.ME` | Methodology | Statistics |
| `physics.hep-th` | High Energy Physics - Theory | Physics |
| `physics.hep-ph` | High Energy Physics - Phenomenology | Physics |
| `quant-ph` | Quantum Physics | Physics |
| `cond-mat.mtrl-sci` | Materials Science | Physics |
| `q-bio.BM` | Biomolecular Structure | Quantitative Biology |
| `q-bio.GN` | Genomics | Quantitative Biology |
| `q-fin.ST` | Statistical Finance | Quantitative Finance |
| `eess.SP` | Signal Processing | Electrical Engineering |
| `eess.AS` | Audio and Speech Processing | Electrical Engineering |

The full taxonomy with all categories is available at [arxiv.org/category_taxonomy](https://arxiv.org/category_taxonomy).

## FAQ

**Can I search for papers by a specific author?**
Yes. Use the `au:` prefix in the search query field. For example, `au:yann lecun` will find papers authored by Yann LeCun. You can combine this with other filters like `au:lecun AND ti:convolutional`.

**What is the difference between `searchQuery` and `category`?**
The `searchQuery` field accepts free-text queries with optional field prefixes and Boolean operators. The `category` field is a convenience filter that appends `AND cat:{value}` to your query automatically. You can use them independently or together. At least one must be provided.

**How do I find the most recent papers in a field?**
Set `sortBy` to `submittedDate` and `sortOrder` to `descending`. Combine with a `category` filter (e.g., `cs.AI`) to get the newest papers in a specific subject area without needing a keyword query.

**Can I get the full text of papers?**
This actor returns the abstract and a direct PDF download link (`pdfUrl`) for each paper. To access the full text content, download the PDF files using the provided URLs. ArXiv's API does not serve full-text content directly.

**Is there a rate limit?**
The ArXiv API requires a minimum 3-second delay between requests. This actor handles rate limiting automatically with a 3.1-second delay between pages. Each page retrieves up to 100 papers, so fetching 500 papers requires about 5 requests and 15 seconds of API wait time.

**How do Boolean operators work in the search query?**
Use uppercase `AND`, `OR`, and `ANDNOT` between field-prefixed terms. For example: `ti:transformer AND au:vaswani` finds papers with "transformer" in the title authored by Vaswani. `abs:reinforcement learning ANDNOT ti:survey` finds RL papers that are not surveys. Operators must be uppercase to be recognized by the ArXiv API.

## Related actors

| Actor | Description | Link |
| --- | --- | --- |
| OpenAlex Research Search | Search the OpenAlex catalog of 250M+ scholarly works, authors, and institutions | [ryanclinton/openalex-research-search](https://apify.com/ryanclinton/openalex-research-search) |
| Crossref Academic Paper Search | Search the Crossref metadata registry for published journal articles with DOIs | [ryanclinton/crossref-paper-search](https://apify.com/ryanclinton/crossref-paper-search) |
| Semantic Scholar Paper Search | Search Semantic Scholar for citation counts, influence scores, and related papers | [ryanclinton/semantic-scholar-search](https://apify.com/ryanclinton/semantic-scholar-search) |
| PubMed Biomedical Literature Search | Find biomedical and life science research papers from the PubMed/MEDLINE database | [ryanclinton/pubmed-research-search](https://apify.com/ryanclinton/pubmed-research-search) |
| CORE Open Access Papers | Search the CORE aggregator for open-access research papers with full-text availability | [ryanclinton/core-academic-search](https://apify.com/ryanclinton/core-academic-search) |
| DBLP Publication Search | Find computer science publications indexed in the DBLP bibliography | [ryanclinton/dblp-publication-search](https://apify.com/ryanclinton/dblp-publication-search) |