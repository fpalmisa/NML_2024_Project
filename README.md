</div> 
<div align="center">
EE-452 Network Machine Learning
</div> 

# Goodreads Bestseller Detector System

## Table of Contents

- [Abstract](#abstract)
- [Project Structure](#project-structure)
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
└── preprocess_v2
└── scratch_Fabio
└── Preprocess
└── quick_look

```
- Simple_Model File : is containing the first model with the book-to-book graph based on ``To Read`` dataset. All the data processing, graph creations and model training are contained in this file.
- Second_Model File : is containing the second model with the book-to-book graph based on ``Ratings`` dataset. All the data processing, graph creations and model training are contained in this file.
- GNN File : is containing the attempt to solve the issues occured in the prediction of labels in GCN models.
- Preprocess : is containing some visualisation and some preprocessing of data
- preprocess_v2 : is containing graph creation of User-to-User, Visualisation, attemps to create Node2Vec
## Dataset Structure
### Goodbooks-10k

This dataset contains six million ratings for ten thousand most popular (with most ratings) books. There are also:

* books marked to read by the users
* book metadata (author, year, etc.) 
* tags/shelves/genres

### Contents

- **ratings.csv** contains ratings sorted by time. It is 69MB and looks like that: 	

- **to_read.csv** provides IDs of the books marked "to read" by each user, as _user_id,book_id_ pairs, sorted by time. There are close to a million pairs.

- **books.csv** has metadata for each book (goodreads IDs, authors, title, average rating, etc.). The metadata have been extracted from goodreads XML files, available in `books_xml`.

- **book_tags.csv** contains tags/shelves/genres assigned by users to books. Tags in this file are represented by their IDs. They are sorted by _goodreads_book_id_ ascending and _count_ descending. 




## Data wrangling
One of the datasets contains all the ratings given by users, amounting to nearly six million ratings. The number of users who have made ratings is $53,424$. This large volume of data needs to be analyzed, and some reductions should be made to handle it more easily. 

The second dataset, ``to read book``, contains less data and includes all the books that users consider interesting and recommend to read. The books dataset consists of all the information about the books, such as authors, ratings, etc. There are 10,000 books in this dataset. 

Additionally, there are two more datasets that are less used in this project: ``tags`` and ``book tag``. 


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


## Report
You can find the report [here](./EE_452_NML_Mheni_Palmisano.pdf)
