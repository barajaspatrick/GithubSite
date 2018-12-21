---
layout: post
title: Tokenize Text Files
---

-------------------
A quick tutorial on building a term document matrix.

##### Useful Terms:

-   Natural Language Processing (NLP) - Programming computers to process
    language.
-   Corpus - A group of all the documents to be used in the program.
-   Tokenize - Cutting up text documents into smaller 'tokens' (words in
    this tutorial).

#### Steps:

1.  Read a large .txt file into R
2.  Concatenate the text file lines into one large string.
3.  Clean the .txt file
4.  Create a Term Document Matrix

------------------------------------------------------------------------

Tm is a text mining package with many useful functions to simplify the
process of creating a Term Document Matrix. You can find the
documentation for the package
[here](https://cran.r-project.org/web/packages/tm/index.html).

    library(tm)

The documents we will use in our TDM are in the directory 'textFile'. We
first create a list of the .txt files and save them in the vector
'textList'.

    Filepath <- "./textFile";
    textList <- list.files(Filepath)

Next we feed in the list of text files into a read function and save the
output into a 'carollBooks' list.

    carollBooks <- list()
    for (i in 1:length(textList)){
            carollBooks[[i]]<- readLines(paste(Filepath,textList[i], sep = "/"))
    }

Right now each book is saved in a list of character vectors. To make
things easier we can extract the elements from each list and save the
contents of each book into a single character vector.

    for (j in 1:length(carollBooks)){
            textList[j] <-  paste(carollBooks[j], collapse = " ")
    }

We can now create our Corpus by passing in our character vectors through
our Corpus function, after running it through our VectorSource Function.
VectorSource converts each of our character vectors into the required
format for Vector to use. VectorSource treats every element we feed into
it as a separate document.

    textList <- VectorSource(textList)
    textList <- Corpus(textList, readerControl =list(readPlain, language="en", load=TRUE))

Once we have our corpus we can create out Term Document Matrix and at
the same time clean up our text by removing stopwords, numbers and
common words.

    term_document_matrix <- TermDocumentMatrix(textList,
                                            control = list(removePunctuation=TRUE,
                                                           stopwords=c(stopwords('english')),
                                                           removeNumbers=TRUE,
                                                           tolower=TRUE))
    term_document_matrix <- as.matrix(term_document_matrix)
    head(term_document_matrix)

    ##             Docs
    ## Terms        1 2
    ##   across     2 1
    ##   actually   1 0
    ##   adventures 2 0
    ##   advice     1 0
    ##   advise     1 0
    ##   alas       2 0
