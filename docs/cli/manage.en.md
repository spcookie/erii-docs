# `erii manage`

Data management TUI for viewing and managing runtime bot data.

```bash
erii manage
```

## Manageable Data Types

| Data Type      | Description                                            |
|:---------------|:-------------------------------------------------------|
| **Memory**     | Fact memory data, with Record, Vector, and Graph views |
| **Profiles**   | User profile data                                      |
| **Memes**      | Meme and catchphrase data                              |
| **Vocabulary** | Learned vocabulary data                                |
| **Summaries**  | Chat summary records                                   |

## Memory Submenu

| View       | Description                                                                            |
|:-----------|:---------------------------------------------------------------------------------------|
| **Record** | Existing memory facts table with list, search, preview, create, edit, and delete       |
| **Vector** | Enter text on the left; view embedding search results and scores on the right          |
| **Graph**  | Enter text on the left; view embedding seeds plus one-hop entity graph nodes and edges |
