All these processes implemented in TFX_Pipeline_for_stock_sentiment.ipynb

# Building a model

I selected [Bert](https://arxiv.org/pdf/1810.04805.pdf) for the backbone of my fined tuned model, one of the SOTA general-purpose models in natural language processing. Here are the pros and cons of Bert:

- Pros

  Very accurate(95% acc in IMDB review supervised problem)

  It made by Google AI team > TF friendly > GCP friendly

  It is available in TFHub and makes it easy to access on backend side

  The high community engagement that leads to implementing a lighter and more accurate version of the model in the      future

- Cons

  It is a large model that can consume a lot of memory and increase the inference time and training time

  It is old (birthday 2018)

The whole process is implemented in the TFX pipeline, the best GCP end to end ML system practice. For GPU requirements for training, I ran my computations of Jupyter notebook on Google Colab.

## Model defining

After downloading the model from TFHub and selecting the final logit layer of the untrainable Bert for the decoding phase, then I added two dense layers (256, 64) for encoding which was followed up by a single node final dense layer with sigmoid activation for the final prediction. 

### Text processing for model input

I use TF built-in function for text processing such as TF tokenizer to reduce the dependency of the model. One of the advantages of TF is that you can embed text processing operations into the model and this empowers you to feed raw text into your model. Because of this wonderful feature, there is no need for text-transform operations on the client side. YES!

## Making Stock dataset TFRecords

We are using the Stock news dataset to train a sentiment model based on the pre-trained BERT model. The dataset provided by Combify. Our ML pipeline can read records, however, it expects only TFRecord files in the data folder. That is the reason why we need to convert our Pandas data frame to TFRecords. It is good to say this procedure can be called by the raw CSV file as it is highly recommended by GCP standards for distributed IO operations in the training phase. With this method, we gain a good utilization in CPU-bound operations and also I/O bound operations from a continuous re-training perspective.



## Exporting Model

After training the model, we exporting a special format of model just for inferencing purposes.

