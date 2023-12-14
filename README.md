# Clinical-Trial-Matching
### Devi Phani Sravanthi Nittala and Pramod Kumar Undrakonda

## Abstract

In the field of healthcare, a critical challenge exists in efficiently connecting patients with appropriate clinical trials that align with their unique medical conditions, demographic factors, and specific criteria.
Clinical trials represent invaluable opportunities for patients to access cutting-edge treatments and therapies, contribute to medical research, and potentially find solutions to their health concerns.
However, the process of matching patients with suitable clinical trials is often complex, time-consuming, and can lead to missed opportunities for both patients and medical researchers.
Our project is an implementation of a model that aims to address this issue and ease the process of patient to trial matching. We have implemented a document-ranking approach to our project, where we use pretrained models finetuned for the purpose of document ranking and then use this model to rank the clincal trials to find the most relevant for a particular clincal case.

## Introduction

The landscape of healthcare is continuously evolving, marked by breakthroughs in medical science,
technological advancements, and a growing emphasis on personalized patient care. At the heart
of this dynamic ecosystem lies the crucial domain of clinical trials, playing an instrumental role in
advancing medical knowledge, testing innovative treatments, and shaping the future of healthcare.
Clinical trials, by design, aim to investigate the safety and efficacy of new drugs, therapies, or
medical interventions. However, a persistent challenge in the realm of clinical research is the efficient
and timely recruitment of eligible participants. 

The traditional methods of patient recruitment for
clinical trials often involve manual screening of medical records, a process susceptible to delays,
inefficiencies, and missed opportunities for patients to access potentially life-changing treatments.
In response to these challenges, the intersection of healthcare and cutting-edge technologies has
given rise to innovative solutions. One such transformative solution is the integration of Natural
Language Processing (NLP) and machine learning into the clinical trial recruitment process. This
project explores the development and implementation of a sophisticated "Clinical Trial Matching"
system, leveraging these advanced technologies to revolutionize the patient recruitment landscape.


## Results

The results can be divided into two parts: first we test the performance of a ClincalBERT model on the MSMARCO dev dataset and second, we view the performance of the model for the purpose of ranking clinical trials.

It is to be noted that since the ClincalBERT model is not directly supported by sentence-tranformers some of the weights are initialized using Mean Pooling during training.

### MSMARCO - Test

The model which had been trained[2] as described above was tested using the evaluation script provided in [2] and also separetely to obtain the various metrics for the model.

|Accuracy | Precision | Recall | Mean Reciprocal Rank | Mean Average Precision | NDCG |
|---------|-----------|--------|----------------------|------------------------|------|
| 0.9290830945558739 | 0.09802292263610315 | 0.9240210124164278 | 0.815079763041796 | 0.815079763041796 | 0.8381790579726461 |

In the above table, the mean reciprocal rank refers to the metric that can evaluate a ranking system by taking the recirpocal of the rank at which the first relevant document is found. The higher the MRR is, the better is the model performance. The next is the mean average precision which is essentially the mean of the average precisions for each query, over the entire corpus. The accuracy of our system is fairly high showing that the model performs well over the test set as well.

### Clincal Trial - Test

We test the same query against the first 20 clincal trials that have been fetched from the clincaltrials.gov for a given condition. For the case shown below, the condition taken was 'heart attack'. We encoded the query and then comapred against the encoding for the Inclusion criteria which dictates a set of conditions that a patient must match in order to be eligible for a clinical study. The query was given as follows:

`A 45 year old with a clinical diagnosis of ST-segment elevation acute myocardial infarction.`

We considered the following models and mentioned below each model is the NCT ID, i.e. the unique ID given to each clinical study, and the cosine similarity obtained w.r.t. the above query.

| Model | Our Model (sravn/msmarco-clincalbert) | sentence-transformers/msmarco-bert-base-dot-v5 | Capreolus/bert-base-msmarco | sentence-transformers/msmarco-MiniLM-L6-cos-v5 |
|---|--|--|--|--|
| NCT ID of 1st ranked trial | NCT01484158 | NCT01484158 | NCT01109225 | NCT01484158 |
|---|--|--|--|--|
| Cosine Similarity | 0.6799761056900024 | 0.9552577137947083 | 0.9649959206581116 | 0.717819094657898|

As we can see in the above results, our model as well as the models trained by the sentence-transformers return the same (and correct) most relevant clincal trial, however, unlike our model, the other had a higher cosine similarity. The model ' Capreolus/bert-base-msmarco' gave another trial as the most relevant trial with a lower cosine similarity. We conclude that our model performs well for the ranking of the clinical trials, however, we must further test it with more detailed patients descriptions. 

(The most relevant clinical trial was: Gait Speed for Predicting Cardiovascular Events After Myocardial Infarction)

## Relevant Links:

HuggingFace model: sravn/msmarco-clincalbert

Clincal Trials Repository: https://clinicaltrials.gov


## References

[1] Reimers, Nils, and Iryna Gurevych. "Sentence-bert: Sentence embeddings using siamese bert-networks." arXiv preprint arXiv:1908.10084 (2019).

[2] MS MARCO https://github.com/UKPLab/sentence-transformers/blob/master/examples/training/ms_marco/

[3] Nguyen, Tri, et al. "MS MARCO: A human generated machine reading comprehension dataset." choice 2640 (2016): 660.

[4] Henderson, Matthew, et al. "Efficient natural language response suggestion for smart reply." arXiv preprint arXiv:1705.00652 (2017).

[5] Gao, Junyi, et al. "COMPOSE: Cross-modal pseudo-siamese network for patient trial matching." Proceedings of the 26th ACM SIGKDD international conference on knowledge discovery & data mining. 2020. https://github.com/v1xerunt/COMPOSE/tree/master
