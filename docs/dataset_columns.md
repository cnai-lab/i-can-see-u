# Main Parquet Dataset Columns

One row represents one post. This table covers used input fields and columns
added to the main Parquet dataset by Steps 1–2.

Patterns: `{entity}` = `hashtags`, `account_mentions`, `urls`, or `ner_entities`;
`{window}` = `12H` or `3D`; `{k}` = selected K-Means cluster count;
`{size}` = BERTopic minimum topic size (e.g., `256`).

Types describe decoded values. `JSON list[...]` is a string containing a JSON
array. Input entity lists may also be JSON strings; Step 1 normalizes entities
and serializes lists as JSON.

| Column name | When added | Type | Short explanation |
|---|---|---|---|
| `postid` | Input | `str` | Post identifier. |
| `accountid` | Input | `str` | Author identifier. |
| `post_time` | Input | `datetime` | Post timestamp. |
| `post_text` | Input | `str` | Original post text. |
| `post_language` | Input | `str` | Source language; Step 1 maps it to an NLLB code such as `eng_Latn`. |
| `is_control` | Input | `bool` | Ground truth: `True`/`1` means control; `False`/`0` means IO. |
| `hashtags` | Input | `list[str]` | Hashtags in the post. |
| `account_mentions` | Input | `list[str]` | Accounts mentioned in the post. |
| `urls` | Input | `list[str]` | URLs in the post. |
| `in_reply_to_accountid` | Input | `str` (nullable) | Account being replied to. |
| `reposted_accountid` | Input | `str` (nullable) | Original author of a repost. |
| `is_repost` | Input | `bool` | Whether the post is a repost. |
| `reposted_postid` | Input | `str` (nullable) | Original post identifier for a repost. |
| `clean_text_with_entities` | Step 1 | `str` | Lowercased text with shortened URLs and labeled hashtag, mention, and emoji tokens. |
| `clean_text_without_enteties` | Step 1 | `str` | Lowercased text with URL/user placeholders and hashtag markers removed; the spelling matches the code. |
| `translated_post` | Step 1 | `str` | English text used for NER and embeddings. |
| `ner_entities` | Step 1 | `list[str]` | Named entities extracted from translated text. |
| `{entity}_reuse_{window}_accountids` | Step 1 | `JSON list[str]` | Unique accounts with strictly earlier posts sharing an entity within the lookback window. |
| `{entity}_reuse_{window}_postids` | Step 1, optional | `JSON list[str]` | Matching earlier post IDs; created only when `include_entity_reuse_post_ids=True`. |
| `K_Means_topic_{k}` | Step 1 | `int` | K-Means topic label. |
| `posting_same_topic_K_Means_topic_{k}_{window}_accountids` | Step 1 | `JSON list[str]` | Unique accounts with strictly earlier posts in the same K-Means topic within the window. |
| `BERTopic_topic_{size}` | Step 2 | `int` | BERTopic topic identifier; `-1` denotes an outlier topic when present. |
| `BERTopic_prob_{size}` | Step 2 | `float` | Assignment score returned by BERTopic for the post's topic. |
| `BERTopic_topic_{size}_top_words` | Step 2 | `JSON list[str]` | Representative words for the assigned topic. |
| `posting_same_topic_BERTopic_topic_{size}_{window}_accountids` | Step 2 | `JSON list[str]` | Unique accounts with strictly earlier posts in the same BERTopic topic within the window. |
