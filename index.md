
* https://gpt-index.readthedocs.io/en/stable/getting_started/concepts.html

1. RAG -> retrieval augmented generation -> give memory to llm

  a. Two stages

    i. Indexing -> Storing

    ii. Querying -> Asking stored data

  b. Querying -> 3 processes

    i. Retriever

    ii. Node Postprocessor

    iii. Response Synthesizer <-- What I'm reading

2. Response Synthesizer -> text chunks + query = response

  a. after -> Nodes are retrieved & Preprocessing done

  b. Types of reponse synthesizer

    i. Refine --> Use each chunk to 'refine' answer

    ii. Compact --> Concat chunks for lesser llm calls

    iii. Tree_summarizer --> Recursively call llm to summarise chunks. Chunks feed into each other. Feed forward of sorts.

    iv. simple_summarise --> Truncate chunks to fit

    v. no_text --> Inspecting responses -> Returns nodes that would've been sent.

    vi. accumulate --> chunks.map(+query).map(llm_summary).accumulate(concat , "")

    vii. compact_accumulate --> concat chunks -> accumulate

  * refine and compact -> structured_answer_filtering -> filters out irrlevant nodes

* Prompts are taken from llama_index. Check prompts. Interesting

* https://gpt-index.readthedocs.io/en/stable/core_modules/query_modules/response_synthesizers/modules.html -> kwargs available

  - response_mode -> type of synthesizer

  - service_context -> LLM + settings

  - text_qa_template && refine_template -> prompts used

  - use async -> Only for Tree_summarizer mode. Async build tree

  - streaming -> Bool

  - structured_answer_filtering -> given above

* Can give source nodes in synthesize func

Things I'm looking at

1. BaseSynthesizer

  1. ServiceContext

    1. CallbackManager -> Manages events in llama

    2. def from_defaults -> Default options. Gold for seeing stuff

    3. from_service_context -> Not sure. Read

---

Note -> predictor is sync, so called func would be sync. apredictor is async. This is relevant when -> response_synthesizers/accumulate.py/ + line: 97-100


