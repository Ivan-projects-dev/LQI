#ML #DB #Math 
**[[Vector]] DB** is a specialized type of DB designed to store, index & search high dimensional [[Vector]] representations of data known as [[Embeddings]]. Unlike traditional DBs that rely on exact matches [[Vector]] DBs use similarity search techh as Cosine similarity or Euclidean distance to find items that are semantically or visually similar.
![3333](https://media.geeksforgeeks.org/wp-content/uploads/20250814034914997961/3333.webp)
![vector](https://media.geeksforgeeks.org/wp-content/uploads/20250818181337924930/vector.webp)
![frame_3078.webp](https://media.geeksforgeeks.org/wp-content/uploads/20250818181142524112/frame_3078.webp)![frame_3078.webp](https://media.geeksforgeeks.org/wp-content/uploads/20250818181142524112/frame_3078.webp)
- [[Embeddings]] work by converting raw data like text, images or audio into dense num vectors that preserve meaning & relationships.
- First the input is processed through a model such as a transformer for text or a CNN for images to extract key features.
- These features are then encoded into fixed length vectors in a high dimensional space where similar items are positioned close together & dissimilar ones are farther apart.
- This spatial arrangement allows similarity to be measured mathematically enabling apps like search, recommendations & classification to operate based on meaning rather than exact matches.

Popular [[Vector]] DBs
- **Pinecone:** Fully managed, cloud native [[Vector]] DB with high scalability & low Latency search.
- **Weaviate:** Open source, supports hybrid (keyword + [[Vector]]) search & offers built in machine learning modules.
- **Milvus:** Highly scalable, open source DB optimized for large scale similarity search.
- **Qdrant:** Open source, focuses on high recall, performance & ease of integration with AI apps.
- **ChromaDB:** Lightweight, dev friendly [[Vector]] DB often used in LLM powered apps.

**Usage**:
- **Image & Video Search:** Finds visually similar media from a DB. Feature [[Embeddings]] are extracted from media files & stored in the [[Vector]] DB. When a new image or frame is queried, the system quickly retrieves the most visually similar results.
- **Question-answering Systems:**etrieves the most relevant infoarge knowledge bases. The system embeds both queries & stored text then compares their vectors to find the closest match. This improves accuracy compared to simple keyword matching.
- **Cross Lingual IR:** Supports matching across multiple langs using multilingual [[Embeddings]]. Text in different langs is converted into a shared embedding space. This allows searching in one lang & retrieving relevant results in another.
- **Fraud & Anomaly Detection:** Identifies unusual patterns by comparing [[Embeddings]] with normal data. The DB can store [[Embeddings]] of typical behavior & detect deviations. This helps in early identification of fraudulent or suspicious activities.