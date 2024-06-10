<div align="center">
Ecole Polytechnique Fédérale de Lausanne
</div> 
<div align="center">
EE-452 Network Machine Learning
</div> 

# Goodreads Bestseller Detector System

## Table of Contents

- [Abstract](#abstract)
- [Project Structure](#project-structure)
- [Dataset Structure](#dataset-structure)
- [Data Wrangling](#data-wrangling)
- [Models](#models)
- [Results](#results)
- [Report](#report)

## Abstract 
This project investigates predicting book bestseller status using various machine learning models and graph-based features, highlighting the potential of linear models, the challenges faced by neural networks, and the benefits of incorporating statistical book features.

## Project structure
```
.
├── README.md
├── data
│   ├── books_xml
│   ├── samples
        └── book_tags.csv
        └── book.csv
        └── ratings.csv
        └── tags.csv
        └── to_read.csv
│   └── book_tags.csv
    └── books.csv
    └── node_features_extended.csv
    └── node_features.csv
    └── ratings
    └── tags.csv
    └── to_read.csv
└── Simple_Model
└── Second_model
└── GNN
└── main_v2
└── scratch_Fabio
└── quick_look

```
## Dataset Structure
# goodbooks-10k

This dataset contains six million ratings for ten thousand most popular (with most ratings) books. There are also:

* books marked to read by the users
* book metadata (author, year, etc.) 
* tags/shelves/genres

## Contents

**ratings.csv** contains ratings sorted by time. It is 69MB and looks like that:

	user_id,book_id,rating
	1,258,5
	2,4081,4
	2,260,5
	2,9296,5
	2,2318,3
	
Ratings go from one to five. Both book IDs and user IDs are contiguous. For books, they are 1-10000, for users, 1-53424. 	

**to_read.csv** provides IDs of the books marked "to read" by each user, as _user_id,book_id_ pairs, sorted by time. There are close to a million pairs.

**books.csv** has metadata for each book (goodreads IDs, authors, title, average rating, etc.). The metadata have been extracted from goodreads XML files, available in `books_xml`.

**book_tags.csv** contains tags/shelves/genres assigned by users to books. Tags in this file are represented by their IDs. They are sorted by _goodreads_book_id_ ascending and _count_ descending. 

In raw XML files, tags look like this:

	<popular_shelves>
		<shelf name="science-fiction" count="833"/>
		<shelf name="fantasy" count="543"/>
		<shelf name="sci-fi" count="542"/>
		...
		<shelf name="for-fun" count="8"/>
		<shelf name="all-time-favorites" count="8"/>
		<shelf name="science-fiction-and-fantasy" count="7"/>	
	</popular_shelves>

Here, each tag/shelf is given an ID. **tags.csv** translates tag IDs to names.

Some of these files are quite large, so GitHub won't show their contents online. See `samples` for smaller CSV snippets.

### goodreads IDs

Each book may have many editions.  _goodreads_book_id_ and _best_book_id_ generally point to the most popular edition of a given book, while goodreads  _work_id_ refers to the book in the abstract sense. 

You can use the goodreads book and work IDs to create URLs as follows:

https://www.goodreads.com/book/show/2767052   
https://www.goodreads.com/work/editions/2792775  

Note that _book_id_ in **ratings.csv** and **to_read.csv** maps to _work_id_, not to _goodreads_book_id_, meaning that ratings for different editions are aggregated.

## Data wrangling
One of the datasets contains all the ratings given by users, amounting to nearly six million ratings. The number of users who have made ratings is $53,424$. This large volume of data needs to be analyzed, and some reductions should be made to handle it more easily. 

The second dataset, ``to read book", contains less data and includes all the books that users consider interesting and recommend to read. The books dataset consists of all the information about the books, such as authors, ratings, etc. There are 10,000 books in this dataset. 

Additionally, there are two more datasets that are less used in this project: ``tags" and ``book tag". 

## Models
Several models are proposed for these project. Most of them are Linear models. A simple GNN model is also implemented.

## Results

### Graph one Book-to-Book Trivial Case
| Model                 | Features  |  Accuracy | F1-score |
|-----------------------|-----------|-----------|----------|
| Logistic Regression   | All       | 0.744     | 0.803    |
| Logistic Regression   | Best      | 0.716     | 0.813    |
| Random Forest         | Best      | 0.739     | 0.805    |
| Random Forest         | All       | 0.748     | 0.814    |
| SVM                   | All       | 0.718     | 0.807    |

### Graph one Book-to-Book only with Graph properties
| Model                 | Features             |  Accuracy | F1-score |
|-----------------------|----------------------|-----------|----------|
| Logistic Regression   | Some properties      | 0.746     | 0.683    |
| Logistic Regression   | All properties       | 0.746     | 0.683    |
| Logistic Regression   | Best features        | 0.754     | 0.692    |
| Simpler Neural Model  | Best features (2L)   | 0.747     | 0.650    |
| Simpler Neural Model  | Best features (3L)   | 0.754     | 0.690    |

### Graph one Book-to-Book only with Graph properties (repeated section title corrected)
| Model                 | Features             |  Accuracy | F1-score |
|-----------------------|----------------------|-----------|----------|
| Logistic Regression   | All features         | 0.809     | 0.775    |
| Logistic Regression   | Best features        | 0.821     | 0.787    |
| Random Forest         | Best features        | 0.829     | 0.800    |
| Random Forest         | Grid Search          | 0.830     | 0.799    |
| Random Forest         | Best + Standardize   | 0.754     | 0.690    |
| MLP                   | All + Standardize    | 0.755     | 0.687    |
| GCN                   | All                  | 0.443     | 0.613    |


### Report
You can find the report [here](./EE_452_NML_Mheni_Palmisano.pdf)
