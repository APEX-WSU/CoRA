# CoRA dataset
The subset of CoRA found [here](https://sites.google.com/site/semanticbasedregularization/home/software/experiments_on_cora) in JSON format, 
with classification data taken from [Andrew McCallum's original dataset](https://people.cs.umass.edu/~mccallum/data.html).

## Data files:
| File                         |  Schema                               |Description                                                                                          |
|------------------------------|---------------------------------------|----------------------------------------------------------------------|
| `classification_papers.json` | `{classification: [paper_ids]}`       | paper ids for 81 classifications (10 level 1, 64 level 2, 7 level 3) |
| `paper_authors.json`         | `{paper_id: [author_ids]}`            | author ids for each of 11804 papers.                                 |
| `term_dictionary.json`       | `{term_id: term}`                     | terms for term ids used in `paper_terms.json`.                       |
| `author_names.json`          | `{author_id: author_name}`            | names for author ids.                                                |
| `paper_references.json`      | `[[citing_paper_id, cited_paper_id]]` | directed paper citation.                                             |
| `paper_terms.json`           | `{paper_id: {term_id: tf_idf}}`       | TF-IDF weighted term counts for 9568 terms.                          |

## Python usage examples
### Labeled tf-idf DataFrame:
```python
import pandas as pd
import numpy as np
import json

paper_terms = json.load(open('paper_terms.json'))
terms = json.load(open('term_dictionary.json'))
df = pd.DataFrame.from_dict(paper_terms, 'index', dtype=pd.SparseDtype('float', 0)).fillna(0) 
df.index.name = 'paper_id'
df.columns = [terms[c] for c in df.columns]
```

### NetworkX (2+) paper citation graph
```python
import networkx as nx
import json

citations = json.load(open('paper_references.json'))

g = nx.DiGraph()
g.add_edges_from(author_names)
```
