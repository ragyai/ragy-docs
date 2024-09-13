# ragy-docs
docs for Ragy.ai the web search api for AI/LLMs

## Endpoint:
https://api.ragy.ai/v1/search

## Authentication:
[Signup for an api key](). You can use this key for api authentication with:
- a header parameter: 'Authentication: Bearer YOUR_API_KEY' (recommended)
- a url parameter: &api_key=YOUR_API_KEY

Example api search request:
https://api.ragy.ai/v1/search?api_key=YOUR_API_KEY&q=what%20is%20rag&lang=en&country=US&max_snippets_length=10000

## Request parameters:
- **q**: the query like 'what is rag' OR just 'rag'
- **lang**: the language for the search (ISO 3166-1 alpha-2)
- **country**: country for the search (ISO 639-1)
- **accept_language**: if no lang is set, you can add the users header Accept-Language from which we extract the first lang
- **client_ip**: in no country is set, we use the ip's country (from the client_ip variable, X-Forwarded-For header or requesting ip)
- **timeout**: after this timeout (in seconds) the response is returned when there is enough data (default 2). Use any value from 0.5 (speedy but no live summaries) to 10 (slow but all webpages that respond in 10 seconds are summarized)
- **max_snippets_length**: the max total length of all snippets (default 10000). If above 0 then unique snippets are added to each web result (a list with unique sentences) 
- **limit**: the maximum number of results (default 50, max 100)
- **add_markdown** *: 1: add a markdown field in the commonmark format, 2: add a cleaned markdown field without images and links
- **add_raw** *: 1: add a raw_content field with all (article) tekst
- **add_reader** *: 1: add a reader field with cleaned html
- **add_html** *: 1: add a html field with the raw html

*\* these are large fields which can only be retrieved live so this slows down. Use only when necessary and with timeout &gt;= 2*

## Response:
The response contains 4 types of results ([view full json example](https://raw.githubusercontent.com/ragyai/ragy-docs/main/search_example.json)):
- **results**: web results with a url, title, description and optional with a content field (about 10 sentences from the webpage). The description/content is uniquely summarized in the snippets field, when max_snippets_length is set above 0
- **qnas**: qna results with a question, answer and link *
- **images**: image results *
- **videos**: video results *
- **related**: related queries *
- **questions**: related questions *

*\* if available*

## FAQ

**What does the Ragy.ai api do?**
For each api query, we retrieve 10 sources with up to 50 web results each. We deduplicate, re-rank and clean those results while we retrieve and summarize the relevant web pages at the same time.

**Which search engines do you use?**
We use our own search engine, content specific sources like wiki and geographical search engines and generic web search engines like Google, Bing and Brave, so in total over 10 sources for each search

**What data do you provide?**
For all results we provide an url, title and description (like most search engines) and a content summary for the most important pages. The description and content field are uniquely summarized in the snippets field (up to max_snippets_length chars) so you can add the snippets easily to an llm.

**How do you summarize?**
We summarize each html page into the most important unique sentences (with the textrank algorithm).

**What's the speed?**
Because everything is done simultaneously, an api call normally takes max 2 seconds (you can set a timeout from 0.5 to 5 seconds).

Example result ([view full json example](https://raw.githubusercontent.com/ragyai/ragy-docs/main/search_example.json)):
```
{
         url: "https://aws.amazon.com/what-is/retrieval-augmented-generation/",
         title: "What is RAG? - Retrieval-Augmented Generation AI Explained - AWS",
         description: "How can AWS support your Retrieval-Augmented Generation requirements? Retrieval-Augmented Generation (RAG) is the process of optimizing the output of a large language model, so it references an authoritative knowledge base outside of its training data sources before generating a response.",
         content: "Retrieval-Augmented Generation (RAG) is the process of optimizing the output of a large language model, so it references an authoritative knowledge base outside of its training data sources before generating a response. The goal is to create bots that can answer user questions in various contexts by cross-referencing authoritative knowledge sources. ...",
         snippets: [
            "Retrieval-Augmented Generation (RAG) is the process of optimizing the output of a large language model, so it references an authoritative knowledge base outside of its training data sources before generating a response.",
            "The goal is to create bots that can answer user questions in various contexts by cross-referencing authoritative knowledge sources.",
            "It redirects the LLM to retrieve relevant information from authoritative, pre-determined knowledge sources.",
            "Even if the original training data sources for an LLM are suitable for your needs, it is challenging to maintain relevancy.",
            "With RAG, an information retrieval component is introduced that utilizes the user input to first pull information from a new data source.",
            "What is the difference between Retrieval-Augmented Generation and semantic search.",
            "Semantic search enhances RAG results for organizations wanting to add vast external knowledge sources to their LLM applications.",
            "Semantic search technologies can scan large databases of disparate information and retrieve data more accurately.",
            "In contrast, semantic search technologies do all the work of knowledge base preparation so developers don't have to."
         ]
      }
```
See the full response with an api subscription (3.000 free request per month)

[Signup](https://www.ragy.ai/signup) (3.000 free credits a month) (or [Rapidapi](https://rapidapi.com/pschinkel80/api/ragy-search) )
 
[Docs](https://www.ragy.ai/docs)
 
[Website](https://www.ragy.ai/)
