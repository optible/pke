# pke - python keyphrase extraction

pke currently implements the following keyphrase extraction models:

- Unsupervised models
  - SingleRank [(Xiaojun and Jianguo, 2008)][1]
  - TopicRank [(Bougouin, Boudin and Daille, 2013)][2]
  - KP-miner [(El-Beltagy and Rafea, 2010)][3]
  - TF*IDF
- Supervised models
  - Kea

## Requirements

    numpy
    scipy
    nltk
    networkx
    corenlp_parser

To install corenlp_parser:

    pip install git+https://github.com/boudinfl/corenlp_parser.git

## Installation

To install this module:

    pip install git+https://github.com/boudinfl/pke.git

## Usage

Before using some keyphrase extraction algorithms (e.g. TfIdf, KP-Miner), one 
need to compute idf weights from a collection of documents. Such weights can
be computed as:

    from pke import compute_inverse_document_frequency
    from nltk.corpus import stopwords
    from string import punctuation

    # path to the collection of documents
    input_dir = '/path/to/input/documents'

    # path to the idf weights dictionary, saved as a pickle
    output_pkl = '/path/to/output/'

    # stoplist, candidates containing stopwords are filtered
    stoplist = stopwords.words('english') + list(punctuation)

    # compute idf weights
    compute_inverse_document_frequency(input_dir, output_pkl, stoplist=stoplist)

### Unsupervised models

#### SingleRank

    from pke import SingleRank

    # create a SingleRank object
    extractor = SingleRank(input_file='/path/to/input')

    # load the content of the document, here in CoreNLP XML format
    # the use_lemmas parameter allows to choose using CoreNLP lemmas or stems 
    # computed using nltk
    extractor.read_corenlp_document(use_lemmas=False)

    # select the keyphrase candidates, for SingleRank the longest sequences of 
    # nouns and adjectives
    extractor.candidate_selection()

    # weight the candidates using a random walk
    doc.candidate_weighting()

    # print the n-highest (10) scored candidates
    print (u';'.join([u for u, v in doc.get_n_best(n=10)])).encode('utf-8')

#### TfIdf
    

    




[1]: http://aclweb.org/anthology/C08-1122.pdf
[2]: http://aclweb.org/anthology/I13-1062.pdf
[3]: http://aclweb.org/anthology/S10-1041.pdf
