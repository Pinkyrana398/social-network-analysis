# 🔗 LinkedIn Social Network Analysis

An end-to-end social network analysis project built on real LinkedIn connection data of students. The project involves large-scale data cleaning, graph construction, and network analysis using random walks, pruning, and centrality metrics.

> ⚠️ **Note:** Raw data files (CSV connections and adjacency list) are not included in this repository to protect the privacy of individuals. The notebooks contain the full pipeline code.

---

## 📌 Overview

LinkedIn exports connection data as individual CSV files — one per user — containing the names and companies of their connections. This project:

1. **Cleans and standardizes** hundreds of raw CSV/XLSX files into a uniform format
2. **Constructs an adjacency list** representing a graph of ~28,500 student nodes and ~100,700 edges
3. **Applies graph algorithms** (random walks, pruning) to explore the network structure
4. **Derives insights** through centrality metrics (degree, betweenness, closeness)
5. **Visualizes results** using bar plots and network diagrams

---

## 📊 Dataset Stats

| Metric                    | Value                              |
| ------------------------- | ---------------------------------- |
| Total Students (Nodes)    | ~28,500                            |
| Total Connections (Edges) | ~100,700                           |
| Raw Files Processed       | 100+ CSV/XLSX files                |
| Data Source               | LinkedIn public connection exports |

> Raw data is excluded from this repo for privacy. See **Data Format** section below to understand the input structure.

---

## 🗂️ Project Structure

```
social-network-analysis/
│
├── cleanData.ipynb        # Data cleaning pipeline
├── assign_ques.ipynb      # Graph construction, analysis & visualization
├── .gitignore             # Excludes raw CSV data and adjacency list
└── README.md
```

---

## 📁 Data Format (Not Included)

### Input — CSV files (one per student)

Each file is named `Firstname Lastname.csv` and contains their LinkedIn connections:

```
Full Name,Company
Rahul Sharma,TCS
Priya Singh,Infosys
Amit Kumar,Wipro
```

### Intermediate — Adjacency List (`adjacency_list.json`)

After cleaning, connections are modeled as a graph:

```json
{
  "rahul sharma": ["priya singh", "amit kumar", "neha gupta"],
  "priya singh": ["rahul sharma", "neha gupta"],
  "amit kumar": ["rahul sharma"]
}
```

> Names are stored in lowercase for deduplication consistency.

---

## 🧹 Data Cleaning Pipeline (`cleanData.ipynb`)

The raw data required extensive preprocessing before graph construction:

- **File consolidation** — Recursively located all `.csv` and `.xlsx` files across nested subfolders and moved them to a single working directory
- **Format conversion** — Converted `.xlsx` files to `.csv` using Pandas
- **Filename standardization** — Cleaned filenames to a consistent `Firstname Lastname.csv` format using regex; removed macOS artifact files (`._` prefixed)
- **Duplicate removal** — Detected and removed files that would map to the same name post-cleaning
- **Column normalization** — Handled varied column names (`First Name`, `firstname`, etc.), multiple encodings (`utf-8`, `latin1`), and non-standard delimiters (`,`, `;`, `\t`)
- **Full Name generation** — Combined `First Name` + `Last Name` into a single `Full Name` column
- **Invalid row removal** — Dropped rows with blank names or entirely empty records
- **Manual fixes** — Six files with missing `First Name` entries were corrected manually; one file with all data in a single column was reformatted

---

## 🕸️ Graph Construction & Analysis (`assign_ques.ipynb`)

### Graph Construction

After cleaning, an adjacency list was built where:

- Each **node** represents a student (identified by full name)
- Each **edge** represents a mutual LinkedIn connection between two students

### Random Walks

Used to simulate how information or influence might propagate through the network. Random walks help identify:

- Densely connected clusters
- Nodes that act as bridges between communities

### Pruning

Removed low-degree nodes and weak edges to simplify the graph and focus on the core network structure.

### Centrality Metrics

| Metric                     | What It Measures                                             |
| -------------------------- | ------------------------------------------------------------ |
| **Degree Centrality**      | How many direct connections a student has                    |
| **Betweenness Centrality** | How often a student lies on the shortest path between others |
| **Closeness Centrality**   | How quickly a student can reach all others in the network    |

---

## 📈 Visualizations

- Bar plots of top-N students by degree centrality
- Distribution of node degrees across the network
- Subgraph visualizations of highly connected clusters

---

## 🛠️ Tech Stack

| Tool           | Purpose                                         |
| -------------- | ----------------------------------------------- |
| **Python**     | Core language                                   |
| **Pandas**     | Data loading, cleaning, transformation          |
| **NumPy**      | Numerical operations                            |
| **NetworkX**   | Graph construction and algorithm implementation |
| **Matplotlib** | Visualizations and plots                        |

---

## ⚙️ How to Run

### 1. Clone the repository

```bash
git clone https://github.com/Pinkyrana398/social-network-analysis
cd social-network-analysis
```

### 2. Install dependencies

```bash
pip install pandas numpy networkx matplotlib openpyxl
```

### 3. Prepare your data

- Export LinkedIn connections as CSV files (one per student)
- Place them inside a folder named `LinkedIn Data Public/`
- Run `cleanData.ipynb` to process and clean the raw files

### 4. Run the analysis

- Open and run `assign_ques.ipynb` to build the graph and generate insights

---

## 🔍 Key Findings

- A small subset of students (~top 1%) act as major hubs with significantly higher degree centrality
- The network shows **small-world properties** — most students are connected within a few hops
- Betweenness centrality revealed a set of "bridge" students who connect otherwise disconnected communities

---

## 👩‍💻 Author

**Pinky Rana**
[GitHub](https://github.com/Pinkyrana398)

---

## 📄 License

This project is for educational purposes. The underlying connection data belongs to the respective LinkedIn users and is not distributed with this repository.
