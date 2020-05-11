# **Information Extraction application using NLP features and techniques**

CS 6320 – Natural Language Processing

Spring 2020

Sagar Patel [SBP170003]

Parva Shah [PKS180000]

## Problem Description

Representing the information of text entities of interest and relation between them using NLP features and techniques like Information Extraction (IE), identifying and extracting templates from a text corpus. Information Extraction is useful in some of the real-time applications like question answering system, contact information search and removal of noisy data. However, the complexity of natural language can make it very difficult to access the information from the unstructured text. The goal of Information Extraction is to gain knowledge like entity relations by asking questions such as &quot;Is this location a part of some location?&quot; or &quot;who is employed by what company?&quot;.

The project talks about extracting templates for _Work_, _Buy_ and _Part Of_. _Work_ template refers to a person employed in a company at a desired position_. Buy_ template extracts information about the transaction between the source and buyer. Part Of template extracts information regarding the role of the relationship between two locations. Our focus is to leverage NLP features and techniques to achieve this goal.

## **Proposed Solution**



There are a few sub-tasks to achieve Information Extraction. It includes identifying the type of text corpus, segmentation of corpus, tokenizing the sentences into words, lemmatizing the words to extract lemmas, Part-of-Speech (POS) tagging, extracting hypernyms, hyponyms, and as such other NLP tasks.

![](RackMultipart20200511-4-10ytagt_html_5a1e674c88eae523.jpg)

Information Extraction is done on Wikipedia articles. Due to the large size and complexity of the article, it became hard to perform natural language processing tasks, which makes it is necessary to do sentence segmentation.

_Sentence segmentation is the process of dividing the written text into meaningful units, such as words, sentences, or topics._

Now the next step is to create meaningful sentences from the segmented article, which is done by resolving the pronouns in the sentences using co-reference resolution.

_Coreference resolution is the task of finding all expressions that refer to the same entity in a text._

The Coreference resolution is performed using the AllenNLP library. As the library is unable to handle big chunks of text input, we created batched input to obtain the resolution.

![](RackMultipart20200511-4-10ytagt_html_45cb967d6b51b014.jpg)

To recognize the entities in the sentence, Named Entity Recognizer of SpaCy is used.

_Named-entity recognition (NER) (also known as entity identification, entity chunking and entity extraction) is a subtask of [information extraction](https://en.wikipedia.org/wiki/Information_extraction) that seeks to locate and classify [named entity](https://en.wikipedia.org/wiki/Named_entity) mentioned in [unstructured text](https://en.wikipedia.org/wiki/Unstructured_data) into pre-defined categories such as person names, organizations, locations, [medical codes](https://en.wikipedia.org/wiki/Medical_classification), time expressions, quantities, monetary values, percentages, etc._

Buy template:

In this template, we had to extract buyer, item, price, quantity, and source. The sentences for this template is triggered by noun and verbs. There were many variations in the sentence structure for this template. Some of them are as follows:

1. _Buyer_ bought _item_ from _source_ for _price_.
2. _Source_ sold _Item_ to _Buyer_ for _Price_.

The above examples are verb triggered sentences.

Here, the Buyer and Source can be person and organization. Item can be organization and product. Named Entity Recognizer is used to recognize these noun entities as buyer, source, and item.

![](RackMultipart20200511-4-10ytagt_html_afbccecf71fe5bcf.jpg)

To discover the relationship between the entities, we need to know the semantic meaning of the sentence. Sentences triggering the _Buy_ template have roles like agent, experiencer, benefactive, etc. To detect these entities in a sentence Semantic Role Labeling (SRL) is used.

_In natural language processing, semantic role labeling is the process that assigns labels to words or phrases in a sentence that indicate their semantic role in the sentence, such as that of an agent, goal, or result._

![](RackMultipart20200511-4-10ytagt_html_7d93840dc088ec09.jpg)

The SRL is used only when a sentence contains a verb. Each verb sense has numbered arguments like Arg0, Arg1, Arg2, etc. along with modifiers like location, direction, manner, temporal, etc. In our sentences, Arg0 contains buyer which act as an agent, Arg1 tells about the item in the transaction and Arg2 contains the source which act as beneficiary.

Heuristic approaches were applied for identifying right entities from the sentence based on arguments.

Work Template

![](RackMultipart20200511-4-10ytagt_html_eaf9b901c19cecde.jpg)

In this template, we need to extract a person, his/her position, organization, and location. A verb and words with the lemma &quot;be.&quot; trigger work template sentences. For example:-

1. PERSON, POSITION of ORG
2. PERSON (worked|appointed|etc) [prep] POSITION at ORG
3. PERSON [lemma = be]? POSITION [prep] ORG

The above sentences are formed by rule-based grammar. Here phrase structure grammar plays a vital role, especially noun phrases. To detect the noun phrases, we used Constituency Parser.

_A constituency parsed tree displays the syntactic structure of a sentence using a context-free grammar._ ![](RackMultipart20200511-4-10ytagt_html_5e0181746db08c39.jpg)

The main reason to use Constituency Grammar was to get extraneous information and to visualize the entire sentence structure rather than just the grammatical dependencies. Using Constituency Parsing, we obtain noun phrases from which we extract required entities.

Location Template

![](RackMultipart20200511-4-10ytagt_html_efcc27b144ec5166.jpg)

In this template, we need to identify two locations that have _PART OF_ relationship. Named Entity Recognizer is used to determine locations from the sentence.

![](RackMultipart20200511-4-10ytagt_html_b8d14ed42f7281ae.jpg)

Here, dependency parser is used to check the connectivity between two locations. If the sentence is triggered by a verb, a relation is obtained by looking at the subtrees of the verb.

_A dependency parser analyzes the grammatical structure of a sentence, establishing relationships between &quot;head&quot; words and words which modify those heads._

To reinforce this result, we used a public domain dictionary consisting of all the cities, states, and countries.

## **Full Implementation details**

**Programming Tools**

Programming Tools used for this project are as below :

1. Programming platform used – Google Colab
2. For Segmentation, Named Entity Recognition, Dependency parser, Constituency Parser, CoReference Resolution, Semantic Role Labeling – SpaCY, AllenNLP, NLTK

SpaCY - [https://spacy.io/](https://spacy.io/)

AllenNLP - [https://demo.allennlp.org/semantic-role-labeling](https://demo.allennlp.org/semantic-role-labeling)

NLTK - [https://www.nltk.org/](https://www.nltk.org/)

Pandas - [https://pandas.pydata.org/](https://pandas.pydata.org/)

**Architectural Diagram**

The below diagram represents the whole NLP pipeline to extract the template using NLP features and techniques.

![](RackMultipart20200511-4-10ytagt_html_5a1e674c88eae523.jpg) ![](RackMultipart20200511-4-10ytagt_html_48fa734aede6a94b.jpg)

**Results and Error Analysis**

Buy Template:-

![](RackMultipart20200511-4-10ytagt_html_f164dc52b799ec64.jpg)

Work Template:-

![](RackMultipart20200511-4-10ytagt_html_46113f0e5b73882c.jpg)

Part Of Template:- ![](RackMultipart20200511-4-10ytagt_html_1769221024611852.jpg)

**Problems Encountered and Resolution**

In Information Extraction, the main task is to extract entities of interest. For that, entity identification is to be done. We used SpaCY for Named Entity Resolution to identify PERSON, ORGANIZATION, LOCATION, MONEY, PRODUCT and CARDINAL.

Problem: Wrong NER results from SpaCY for PERSON and ORGANIZATION

Solution: Used AllenNLP to identify PERSON and ORG, due to its accurate results. We used results from both the tools for NER.

Problem: During Coreference Resolution, some of the entities were co-referenced and replaced by the whole sentence describing it. And as a result, when the co-referenced sentence was used for template extraction, problems were encountered in constituency parsing and dependency parsing.

Solution: After careful observation of co-reference results, we concluded that if the word is not PRONOUN and entity itself, then no need to change with the co-reference text. This helped us to get pristine and precise results from the constituency and dependency parser.

Problem: Having Determiners in sentences caused the problem in giving noun phrases properly when constituency parsing is performed on sentence.

Solution: Removed all the determiners while doing constituency parsing.

**Pending Issues**

In Buy template, sentences are triggered by a verb and noun words i.e., acquired, buy, sell, sold, acquisition. Our NLP pipeline is limited to extract _buy_ template from sentences that are triggered by verbs.

For example, Amazon&#39;s acquisition of Whole Food Markets in 2012, increased their share value exponentially.

Here, the acquisition is noun and Semantic Role Labelling works on verb argument pattern.

In the _Work_ template, our methodology solely depends on Constituency Parsing. Extracting entities from noun phrases obtained from constituency parsing have no semantic meaning. Therefore, if a sentence contains more than one PERSON entity, along with POSITION and ORGANIZATION, then with just syntactic meaning, it is hard to know the relation of PERSON with other detected entities and this can lead to False Positives.

For example - Steve Jobs and Powell Jobs are the co-founder of Apple, Inc.

In this case, using constituency parsing will give the correct template, but it solely depends on how noun phrases are formed.

**Potential Improvements**

In _Buy_ template, we are using Semantic Role Labelling to get arguments and look for entities in those arguments. But there is still ambiguity in the correctness of QUANTITY identified from the arguments. To detect QUANTITY, we are using NER provided by SpaCY i.e., CARDINAL entities. To further improvise the heuristic approach, we can use dependency parser to check if the quantity is dependent on the item or not. In this way, we can look for the subtrees of an item to check for any CARDINAL entity attached to it.

Also, another potential improvement is to use constituency parsing on the sentences triggered by a noun. In this way, we can work on noun phrases obtained from constituency parsing and extract templates.

In _Work_ template, potential improvement can be made by using SRL and Dependency parser. Using SRL, sentences triggered by verb can be easily extracted to obtain POSITION, PERSON and ORG from arguments. Also, using dependency parser, we can have strong standing on the correction of &quot;Is this position actually refer to him/her?&quot;.

**References**

[http://docs.allennlp.org/master/](http://docs.allennlp.org/master/)

[https://spacy.io/usage/spacy-101](https://spacy.io/usage/spacy-101)

[https://www.kaggle.com/nsharan/h-1b-visa](https://www.kaggle.com/nsharan/h-1b-visa)

[https://www.nltk.org/howto/wordnet.html](https://www.nltk.org/howto/wordnet.html)

[http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.139.3746&amp;rep=rep1&amp;type=pdf](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.139.3746&amp;rep=rep1&amp;type=pdf)
