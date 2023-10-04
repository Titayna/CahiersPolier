# Handwritten Text Recognition (HTR) and the Notebooks of Jean-Henri Polier de Vernand (1715-1791)

Since 2021, the [Archives cantonales vaudoises (ACV)](https://www.vd.ch/toutes-les-autorites/archives-cantonales-vaudoises-acv) and the [Collège des Humanités Digitales (CDH)](https://www.epfl.ch/schools/cdh/fr/) at EPFL – more precisely the [Lausanne Time Machine project](https://www.epfl.ch/schools/cdh/lausanne-time-machine/fr/lausanne-time-machine/) – are collaborating to digitalized the notebooks of Jean-Henri Polier de Vernand. 

As a master's thesis, we applied Handwritten Text Recognition (HTR) technology – using TensorFlow, [Transkribus](https://readcoop.eu/transkribus/) and [HTR-Flor](https://github.com/arthurflor23/handwritten-text-recognition) – to the 26'300 pages that make up the notebooks of Lausanne's bailiff lieutenant. 

This was a part of my Master degree in [Digital Humanities](https://www.unil.ch/lettres/fr/home/menuinst/formations/master-en-humanites-numeriques.html) and [History](https://www.unil.ch/hist/fr/home/menuinst/formations/maitrise-universitaire-1.html) at the University of Lausanne in Switzerland. 

This git presents both the results of this research and the methodology used to transform this valuable source of Lausanne's eighteenth-century history into a digital format.

*The results of the HTR process can be find [here]().* <!-- AJOUTER LIEN RELATIF -->

Table of Contents
=================
* [Jean-Henri Polier de Vernand (1715-1791)](#jean-henri-polier-de-vernand-1715-1791)
* [Methodology](#methodology)
    * [Training set with Transkribus](#training-set-with-transkribus)
    * [HTR-Flor by Arthur Flor](#htr-flor-by-arthur-flor)
* [Results](#results)


## Jean-Henri Polier de Vernand (1715-1791)
Lausanne in 1715 and served there from 1754 until his death in 1791 as lieutenant baillival of Lausanne, thus acting as a substitute for the Bernese bailli [^1]. A member of two Lausanne Councils, the Baillivale Court, the Court of Fiefs, the Criminal Court of the Castle, the Court of the Chapter and a member of the Court of the Rue de Bourg through his status as a landowner, he was one of the most important figures in Lausanne society of his time[^2]. Moreover, Polier kept numerous notebooks from 1754 until his death, in which he methodically transcribed his personal day-to-day life on more than 26,300 pages, thereby creating one of the most important documents in the history of Lausanne[^3].

*For more information, a copy of my master’s thesis in French can be found [here](Master_thesis_Kauffmann_2023.pdf).* 

[^1]: Abetel Emmanuel, « Polier de Vernand, Jean-Henri », [hls-dhs-dss.ch](https://hls-dhs-dss.ch/articles/017839/2009-04-20/), consulté le 03.03.2023.
[^2]: Morren Pierre, La vie lausannoise au XVIIIe siècle: d’après Jean-Henri Polier de Vernand, lieutenant baillival, Genève : Labor et Fides, 1970
[^3]: Abetel Emmanuel, « Polier de Vernand, Jean-Henri »…, [art. cit.](https://hls-dhs-dss.ch/articles/017839/2009-04-20/)


## Methodology 

### Training set with Transkribus 

In order to develop an automatic handwriting recognition model, we had to make a number of transcriptions of Polier de Vernand's notebooks in order to generate data for training the model. To do this, we used Transkribus, which allows us to transcribe documents free of charge and to analyse document layout in order to detect text segmentation.

This data set must reflect a variety of page layouts, vocabulary and writing styles, either by selecting specific pages or by selecting pages at regular intervals. Thus, to build our data set for training our model, we made a selection both for specific pages in certain notebooks with particular features or the presence of a lot of numbers for example but also for certain pages at intervals of 10 pages.

| Header 1 | Header 2 | Header 3 | Header 4 |
|:---------:|:---------:|:---------:|:---------:|
| Centered Cell 1 | Centered Cell 2 | Centered Cell 3 | Centered Cell 4 |
| Centered Cell 5 | Centered Cell 6 | Centered Cell 7 | Centered Cell 8 |



### Sample 

<!-- Mettre un exemple de reconnaissance des pages du cahiers  -->

### HTR-Flor by Arthur Flor 
<!-- Preentrainement sur Bentham Data Set -->

## Results 


