# Handwritten Text Recognition (HTR) and the Notebooks of Jean-Henri Polier de Vernand (1715-1791)

Since 2021, the [Archives cantonales vaudoises (ACV)](https://www.vd.ch/toutes-les-autorites/archives-cantonales-vaudoises-acv) and the [Collège des Humanités Digitales (CDH)](https://www.epfl.ch/schools/cdh/fr/) at EPFL – more precisely the [Lausanne Time Machine project](https://www.epfl.ch/schools/cdh/lausanne-time-machine/fr/lausanne-time-machine/) – are collaborating to digitalized the notebooks of Jean-Henri Polier de Vernand. 

As a master's thesis, we applied Handwritten Text Recognition (HTR) technology – using *TensorFlow*, [*Transkribus*](https://readcoop.eu/transkribus/) and [*HTR-Flor*](https://github.com/arthurflor23/handwritten-text-recognition) – to the 26'300 pages that make up the notebooks of Lausanne's bailiff lieutenant. 

This was a part of my Master degree in [Digital Humanities](https://www.unil.ch/lettres/fr/home/menuinst/formations/master-en-humanites-numeriques.html) and [History](https://www.unil.ch/hist/fr/home/menuinst/formations/maitrise-universitaire-1.html) at the University of Lausanne in Switzerland. 

This git presents both the results of this research and the methodology used to transform this valuable source of Lausanne's eighteenth-century history into a digital format.

As part of our evaluation of this automatic handwriting recognition model, we assessed the *Character Error Rate* (CER). The CER measures the minimum number of character modification operations required to align a word with the *ground truth*. 

Our achievement is highlighted by a **CER of just 8.78%**, showcasing the precision of our model in preserving the historical content. As a general guideline, a CER below 10% is considered usable, and below 5% is deemed very good, with the remaining errors typically attributed to rare or unknown words. This achievement underscores the efficacy of our approach in transcribing and conserving the invaluable insights embedded within these historical documents.


> *The results of the HTR process can be find [here](polier_notebook_json).* 

Table of Contents
=================
* [Jean-Henri Polier de Vernand (1715-1791)](#jean-henri-polier-de-vernand-1715-1791)
* [Methodology](#methodology)
    * [Training set with Transkribus](#training-set-with-transkribus)
        * [From *HTR-Flor* to *JSON*](#from-htr-flor-to-json)
    * [HTR-Flor by Arthur Flor](#htr-flor-by-arthur-flor)
* [Results](#results)


## Jean-Henri Polier de Vernand (1715-1791)
Lausanne in 1715 and served there from 1754 until his death in 1791 as lieutenant baillival of Lausanne, thus acting as a substitute for the Bernese bailli [^1]. A member of two Lausanne Councils, the Baillivale Court, the Court of Fiefs, the Criminal Court of the Castle, the Court of the Chapter and a member of the Court of the Rue de Bourg through his status as a landowner, he was one of the most important figures in Lausanne society of his time[^2]. Moreover, Polier kept numerous notebooks from 1754 until his death, in which he methodically transcribed his personal day-to-day life on more than 26,300 pages, thereby creating one of the most important documents in the history of Lausanne[^3].

> *For more information, a copy of my master’s thesis in French can be found [here](Master_thesis_Kauffmann_2023.pdf).* 

[^1]: Abetel Emmanuel, « Polier de Vernand, Jean-Henri », [hls-dhs-dss.ch](https://hls-dhs-dss.ch/articles/017839/2009-04-20/), consulté le 03.03.2023.
[^2]: Morren Pierre, La vie lausannoise au XVIIIe siècle: d’après Jean-Henri Polier de Vernand, lieutenant baillival, Genève : Labor et Fides, 1970
[^3]: Abetel Emmanuel, « Polier de Vernand, Jean-Henri »…, [art. cit.](https://hls-dhs-dss.ch/articles/017839/2009-04-20/)


## Methodology 

### Training set with Transkribus 

In order to develop an automatic handwriting recognition model, we had to make a number of transcriptions of Polier de Vernand's notebooks in order to generate data for training the model. To do this, we used [Transkribus](https://readcoop.eu/transkribus/), which allows us to transcribe documents free of charge and to analyse document layout in order to detect text segmentation.

This data set must reflect a variety of page layouts, vocabulary and writing styles, either by selecting specific pages or by selecting pages at regular intervals. Thus, to build our data set for training our model, we made a selection both for specific pages in certain notebooks with particular features or the presence of a lot of numbers for example but also for certain pages at intervals of 10 pages.

> **Table I :** Total: 40 digitised pages transcribed (double page handwritten notebook)

| Notebook number : | Page(s): | Notebook number : | Page(s):  |
|:---------:|:---------:|:---------:|:---------:|
| 001 | 4-5-6-49 | 090 | 10 |
| 010 | 10-20 | 100 | 10-30-50 |
| 020 | 4-5-6-7-8 | 110 | 8 |
| 040 | 7-8-41-51 | 125 | 50 |
| 050 | 10 | 145 | 20-30 |
| 060 | 10-20-30-40 | 155 | 10-20 |
| 070 | 10 | 160 | 10-19-28-41 |
| 080 | 10-30-50| 185 | 10-20 |

At the start of this project, Transkribus seemed perfectly suited to the task of creating training data and using it to create a prediction model. However, since at least 2021, Transkribus has set a limit on the number of pages that can be transcribed free of charge, which meant that we were unable to cover the 26,300 pages written by Polier within a reasonable budget. The software currently offers the option of applying an HTR template to 500 handwritten pages free of charge, followed by a fixed price starting at €18 for 120 credits and €2,160 for 10,000 credits - 1 credit corresponding to 1 handwritten page.

For this reason, we turned to the [*HTR-Flor++*](#htr-flor-by-arthur-flor) text recognition model which, although not as easy to use for a user less accustomed to programming language - one of the main advantages of Transkribus - enabled us to obtain similar results. 

#### From *Transkribus* to *HTR-Flor*
To convert the transcriptions made on Transkribus into our handwriting interpretation model, we exported the segmentation - i.e. the text linked to these images - from the platform in XML format. We then used a python script ([From_Transkribus_to_HTR_Flor](From_Transkribus_to_HTR_Flor.ipynb)) to load the image, extract its name and obtain the notebook number and page number of the source, then read the XML file generated by Transkribus for that page. 

It then goes back to each region of text detected on each page using layout analysis and for each of them it obtains the co-ordinates of the outline of the region of text and the associated baseline and slightly increases the margin of pixels around the line and then straightens it by following the baseline - Polier sometimes writes at an angle on his notebook. Each segment is then associated with the transcription made on Transkribus.

Finally, partitions had to be created for the training, validation and test sets from a list of elements. The training set is used to train the model, the validation set is used to evaluate the model's performance and parameter settings during training, and the test set is used to evaluate the model's final performance once training and settings have been completed. In this case, the proportion is 10% for validation and 20% for testing, which means that the remaining 70% will be used for training.

Handwriting recognition is then performed on the [script proposed by Arthur Flor de Sousa Neto](https://github.com/arthurflor23/handwritten-text-recognition/blob/master/src/tutorial.ipynb), which enables a handwritten text recognition model to be trained using TensorFlow - an open source machine learning and data processing library created by Google - on a GPU device that requires the use of Google Colab, an online platform offered by the company of the same name that enables high-level calculations to be performed for machine learning tasks.

### *HTR-Flor* by Arthur Flor 
<!-- Preentrainement sur Bentham Data Set -->

> **Illustration I :** Example of transcription after learning from training data
![Example of transcription after learning from training data](Sample_1.png)

The script is used in two parts: one for training the model on the transcribed pages and one for inference for the rest of the notebooks. To develop the model, we also used data from the tranScriptorium Bentham project. This set of transcripts from the collection of Bentham manuscripts written by the philosopher of the same name were created as part of a competition organised at the ICFHR 2014 conference to evaluate the automatic recognition of handwritten texts.

In our case of the notebooks of Jean Henri Polier de Vernand, the use of transcript data from the Bentham collection not only made it possible to obtain a lower CER, it is also useful since the lieutenant baillival sometimes wrote in English, whether in the case of writing reading notes on English newspapers, or when he did not wish to be able to be read by his servants. 

During this model training phase, the script takes into account all the known data to learn how to recognise character patterns and set up a word dictionary, and evaluates the model by comparing the transcription provided with the values detected by the model. 

Once the model has been trained, the inference phase of automatic text recognition takes place. To do this, we first had to assemble all of Polier de Vernand's notebooks into a format that could be recognised by the HTR-Flor++ script. Arthur Flor de Sousa Neto's program converts Transkribus data into hdf5 files, to which the trained model can then be applied. 

The model is then used to predict the segments created by the layout analysis carried out on Transkribus and thus detect symbols, whether letters or punctuation. In this way, we were able to predict the entirety of the notebooks using the model trained on the transcriptions we had made, as well as the Bentham model.

The predicted data is then supplied in a text file for each notebook, which then needs to be put back into the order of the detections made by Transkribus. To do this, we used the Python script [From_HTR_Flor_to_JSON](From_HTR_Flor_to_JSON.ipynb). It allows us to create JSON files containing the predictions of the signatures in continuous text, while retaining the information about the signature number and the page number of the transcription. The order is reconstituted thanks to the file created when using the script presented in Appendix I and therefore the analysis carried out by Transkribus. 

## From *HTR-Flor* to *JSON* 

> **Illustration II :** Extract from page 3 of notebook no. 93
![Extract from page 3 of notebook no. 93](Sample_2.png)

> Result in a JSON file
``` json
[
    {
        "cahier_n": 91,
        "page_n": 3,
        "transcription": "mercredi 1 avril ; 1778 \n du mercredi 1er avril ;, \n mon frere a la haye; \n jattendois aver bien de l'impatience mon cher frere \n votre lettre du 7 du mois passe, je suis afflige plus \n que je ne puis vous le dine des tristes nouvelles que \n lus donnez de l'etat de votre sante, je sousse in \n finiment de vos maux & prie le ciel avec ardeur de \n les adoucir, je vais languir jusqu'a ce que vous avi \n siez de votre arrivee a la haye. vous demand. \n rais la place de me faire ecrire par le secretaire de \n mr de, pour peu qu vous soyez fatique d't \n vaquer vous meme ; je voudrois pouvoir esperer \n que la douce chaleur du printemps donnera un \n peu de jeu a ves membres & dissipera vos fluxions \n mais lorsque le vent d'est soufler a avec rigueur \n n'y aura t'il pas moyen de vous dispenser d'etre \n present a ces longr exercices vous avez ete si \n souvent de tour qu'il seroit juste de laisser une \n partie du travail a ceux qui ont moins apereque \n vous ; nous venons d'avoir une commolion \n agreable & triste dans la parente; la fille de mr. le \n don, aimable, douee de beaucoup desprit & de \n pande tulenr a suljuque depuis quelques mois \n de sentimens profonds destime & d'amour un jeu \n n: seigneur anglais pair d'irlande sous le titre \n de gatxay, aye de 20 ans, ayant avec lui mne \n espece de gouverneur du pays, momme le minf \n combe; e jeune seigneur pour legitimer se \n passion ; parl. sacrement il s'est fait ecouter \n en a voulu excopter de sa minorite on a miste \n sur le consentement de les parant, l'epousema op \n pes qu'il ne pouvoit sarreter a ces lonqueurs \n qu'il etois trop emzlamme pour attendre,; il a \n bien d illu ceder a des instances si fortes ; les \n cennes ces velles qui se sont melees de cette \n faure n'ont pas trop bien combine taut ola \n dim: 2 mars a 6 h 1/2 du soir, mylard apres \n "
    }
]
```

The notebooks are then available on this repository in [JSON format](polier_json). A file has been created for each notebook, based on the page number assigned by the [Archives cantonales vaudoises (ACV)](https://www.vd.ch/toutes-les-autorites/archives-cantonales-vaudoises-acv). Information about the transcribed page is available for each one. These page numbers correspond to the PDF files resulting from the digitisation of the Polier notebooks. 


