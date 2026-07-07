[Arxiv Paper Search](https://apify.com/gentle_cloud/arxiv-paper-search?fpr=data)

Search and extract academic papers from [ArXiv](https://arxiv.org), the world's largest open-access repository of scientific papers. Retrieve full metadata including title, authors, abstract, categories, publication dates, and PDF download links.

## 🔍 Features

- **Keyword Search** — Search across all fields (title, abstract, comments, journal ref)
- **Author Filter** — Find all papers by a specific researcher
- **Category Filter** — Browse papers in specific ArXiv categories (cs.AI, stat.ML, etc.)
- **ID Lookup** — Retrieve specific papers by their ArXiv IDs
- **Sorting** — Sort by relevance, submission date, or last updated date
- **Pagination** — Automatically paginate through large result sets (up to 5000 papers)
- **Rich Metadata** — Get title, authors, abstract, categories, DOI, journal reference, PDF link, and more

## 📖 How to Use

1. **Choose a search mode**: "Keyword Search" for general queries or "ID Lookup" for specific papers
2. **Enter search criteria**: Fill in query, author, and/or category fields (combine for precise results)
3. **Set limits**: Configure max results and sort order
4. **Run the Actor** and download results as JSON, CSV, or Excel

### Search Query Examples

| Goal | Query | Author | Category |
| --- | --- | --- | --- |
| Find LLM papers | `large language model` |  |  |
| Papers by Hinton |  | `Hinton` |  |
| Recent AI papers |  |  | `cs.AI` |
| Specific author + topic | `transformer` | `Vaswani` |  |
| Category + keyword | `reinforcement learning` |  | `cs.LG` |

### ArXiv Category Codes (Common)

| Code | Field |
| --- | --- |
| `cs.AI` | Artificial Intelligence |
| `cs.CL` | Computation and Language (NLP) |
| `cs.CV` | Computer Vision |
| `cs.LG` | Machine Learning |
| `cs.CR` | Cryptography and Security |
| `stat.ML` | Machine Learning (Statistics) |
| `math.OC` | Optimization and Control |
| `physics.optics` | Optics |
| `q-bio.BM` | Biomolecules |

Full taxonomy: [https://arxiv.org/category_taxonomy](https://arxiv.org/category_taxonomy)

## 📦 Sample Output

```
{
    "arxiv_id": "2303.08774v1",
    "title": "GPT-4 Technical Report",
    "authors": ["OpenAI"],
    "summary": "We report the development of GPT-4, a large-scale, multimodal model...",
    "primary_category": "cs.CL",
    "categories": ["cs.CL", "cs.AI"],
    "published": "2023-03-15T17:58:40Z",
    "updated": "2023-03-15T17:58:40Z",
    "pdf_url": "https://arxiv.org/pdf/2303.08774v1",
    "abs_url": "https://arxiv.org/abs/2303.08774v1",
    "comment": "46 pages",
    "journal_ref": "",
    "doi": "",
    "fetched_at": "2026-03-22T10:00:00+00:00"
}
```

## ⚙️ Notes

- **Rate limiting**: The ArXiv API recommends no more than 1 request per 3 seconds. This Actor automatically handles rate limiting and retries.
- **Result limit**: ArXiv API returns up to 100 results per request. For larger datasets, the Actor paginates automatically (up to 5000 total).
- **No API key required**: The ArXiv API is free and open to everyone.
- **Data freshness**: ArXiv updates daily. New papers are typically indexed within 24 hours of submission.
- **Memory**: 256MB is sufficient for most searches. Use 512MB+ for very large result sets (1000+ papers).