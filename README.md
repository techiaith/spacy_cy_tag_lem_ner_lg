[English Below](#English)

# Model Cymraeg ar gyfer spaCy sy'n tagio rhannau ymadrodd, lemateiddio, adnabod endidau a darparu fectorau geiriau  

Mae'r model hwn yn cynnwys piblinell gyfan o gydrannau er mwyn prosesu'r iaith Gymraeg yn gyfrifiadurol.

## Tocyneiddiwr
Mae'r tocyneiddiwr yn rhannu'r testun yn eiriau ac unedau cyffelyb fel rhifau a nodau atalnodi. Galwn un o'r unedau hyn yn 'docyn'.

```
Dywedodd "Aeth f'annwylyd i'r ffair" heb gyd-destun.
[Dywedodd, ", Aeth, f', annwylyd, i, 'r, ffair, ", heb, gyd-destun, .]
```

## Tagiwr Rhannau Ymadrodd
Mae'r tagiwr rhannau ymadrodd yn dynodi tag penodol ar gyfer pob tocyn yn ôl ei swyddogaeth yn y frawddeg, gan ddefnyddio er enghraifft VERB am ferf a PUNCT ar gyfer nodau atalnodi. Mae'n defnyddio set tagiau Universal Dependencies.

```
Gwelodd gath heb goler na chynffon.
[('Gwelodd', 'VERB'), ('gath', 'NOUN'), ('heb', 'ADP'), ('goler', 'NOUN'), ('na', 'CONJ'), ('chynffon', 'NOUN'), ('.', 'PUNCT')]
```

## Lemateiddiwr
Mae'r lemateiddiwr yn trosi'r geirffurf i'r gair yn ei ffurf gysefin, gan dynnu unrhwy dreigladau neu rediadau berfol/arddodiadol.

```
Gwelodd gath heb goler na chynffon.
['gweld', 'cath', 'heb', 'coler', 'na', 'cynffon', '.']
```

Mae'n defnyddio'r tag rhan ymadrodd er mwyn gallu gwahniaeth rhwng _ceir_ fel ffurf ar _cael_ a _ceir_ fel ffurf ar yr enw _car_ (Saesneg: "cars").

```
Ceir rhai garejys yn gwerthu ceir rhad
[('Ceir', 'cael'), ('rhai', 'rhai'), ('garejys', 'garej'), ('yn', 'yn'), ('gwerthu', 'gwerthu'), ('ceir', 'car'), ('rhad', 'rhad')]
```

## Adnabyddwr Endidau (NER)

Cydran ar yfer adnabod endidau fel enwau pobl, lleoedd a sefydliadau o fewn y testun.

```
Lansiodd David Hill Jones ei gyfrol newydd The Singularity Show yn nigwyddiad y Cyngor Llyfrau yn Aberystwyth yn gynharach heddiw.
[('Lansiodd', ''), ('David', 'PERSON'), ('Hill', 'PERSON'), ('Jones', 'PERSON'), ('ei', ''), ('gyfrol', ''), ('newydd', ''), ('The', 'WORK_OF_ART'), ('Singularity', 'WORK_OF_ART'), ('Show', 'WORK_OF_ART'), ('yn', ''), ('nigwyddiad', ''), ('y', ''), ('Cyngor', 'ORG'), ('Llyfrau', 'ORG'), ('yn', ''), ('Aberystwyth', 'GPE'), ('yn', 'DATE'), ('gynharach', 'DATE'), ('heddiw', 'DATE'), ('.', '')]
```

Dyma'r enididau a ddefnyddwyd:

FAC  -  Adeiladau, meysydd awyr, priffyrdd, pontydd, ac ati.  
ORG  -  Cwmnïau, asiantaethau, sefydliadau, ac ati.  
GPE  -  Gwledydd, dinasoedd, taleithiau.  
LOC  -  Lleoliadau nad ydynt yn GPE, cadwyni o fynyddoedd, cronfeydd dŵr.  
PRODUCT -  Gwrthrychau, cerbydau, bwydydd, ac ati (Nid gwasanaethau.)  
EVENT -  Corwyntoedd a enwir, brwydrau, rhyfeloedd, digwyddiadau chwaraeon, ac ati.  
WORK_OF_ART  -  Teitlau llyfrau, caneuon, ac ati.  
LAW  -  Dogfennau a enwir wedi'u gwneud yn gyfreithiau.  
LANGUAGE -  Unrhyw iaith a enwir.  
DATE  -  Dyddiadau neu gyfnodau absoliwt neu gymharol.  
TIME -  Amseroedd llai na diwrnod.  
PERCENT  -  Canran, gan gynnwys "%".  
MONEY  -  Gwerthoedd ariannol, gan gynnwys uned.  
QUANTITY -  Mesuriadau, yn ôl pwysau neu bellter.  
ORDINAL  -  "cyntaf", "ail", ac ati.  
CARDINAL  -  Rhifolion nad ydynt yn dod o dan fath arall.  

## Fectorau Geiriau
Mae'r model yn cynnwys fectorau geiriau, sef cynrychioliad rhifol o berthynas geirffurfiau a'i gilydd. Maent yn caniatâu i ni ddefnyddio ffwythiannau mathemategol arnynt, er enghraifft i ganfod geirffurfiau sy'n caele u defnyddio mewn ffordd debyg o fewn corpws enfawr.

Dyma'r canlyniad ar gyfer y geirffurfiau tebyg i 'heddiw':

```
['heddiw', 'heddi', 'ddoe', 'yfory', 'nawr', 'heno', 'heddyw', 'Heddiw', 'rŵan', 'fory']
```

Gan ddefnyddio'r cod canlynol:

```
def get_most_similar(query_string, n=10):
    most_similar = nlp.vocab.vectors.most_similar(nlp.vocab[query_string].vector.reshape(1,300), n=n)
    most_similar_wordforms = []
    for key in most_similar[0][0]:
        most_similar_wordforms.append(nlp.vocab[key].orth_)
    return most_similar_wordforms

get_most_similar("heddiw", 10)
```

## Gosod y Model
Dilynwch y cyfarwyddiadau yn https://spacy.io/usage er mwyn gosod llyfrgell spaCy.

Llwythwch y fersiwn diweddaraf o'r model o https://github.com/techiaith/spacy_cy_tag_lem_ner_lg/releases/latest.

Pan fydd wedi lawrlwytho, gosodwch y pecyn drwy ddefnyddio:

`pip install -m enw-ffeil-y-pecyn`

o fewn eich amgylchedd Python.

Gallwch gadarnhau fod y model wedi'i osod drwy deipio:

`python -m spacy info`

Ar hyn o bryd, bydd hefyd yn rhaid i chi gopio'r ffolder `cy` a geir yn https://github.com/techiaith/spacy/tree/welsh/spacy/lang i'ch gosodiad spaCy. 

## Defnyddio
I ddefnyddio'r model o fewn spaCy:

```
import spacy

nlp = spacy.load("spacy_cy_tag_lem_ner_lg")

doc = nlp("The minister, David Jones the member for South Wales East, spoke on behalf of the Welsh Government today in Llandudno.")
for item in [(t, t.lemma_, t.pos_, t.morph, t.ent_type_, t.vector_norm) for t in doc]:
    print (item)
```

Dylai hynny roi'r allbwn canlynol:

```
(Lansiodd, 'lansio', 'VERB', , '', 6.1967916)
(David, 'David', 'PROPN', , 'PERSON', 24.811096)
(Hill, 'Hill', 'PROPN', , 'PERSON', 22.11108)
(Jones, 'Jones', 'PROPN', , 'PERSON', 30.422651)
(ei, 'ei', 'DET', , '', 39.050266)
(gyfrol, 'cyfrol', 'NOUN', , '', 29.970655)
(newydd, 'newydd', 'ADJ', , '', 31.02251)
(The, 'The', 'X', , 'WORK_OF_ART', 29.884811)
(Singularity, 'Singularity', 'X', , 'WORK_OF_ART', 0.0)
(Show, 'Show', 'X', , 'WORK_OF_ART', 17.484062)
(yn, 'yn', 'PART', , '', 26.167881)
(nigwyddiad, 'digwyddiad', 'NOUN', , '', 12.935198)
(y, 'y', 'DET', , '', 30.016222)
(Cyngor, 'Cyngor', 'NOUN', , 'ORG', 33.943195)
(Llyfrau, 'Llyfrau', 'NOUN', , 'ORG', 37.831318)
(yn, 'yn', 'ADP', , '', 26.167881)
(Aberystwyth, 'Aberystwyth', 'PROPN', , 'GPE', 25.005033)
(yn, 'yn', 'PART', , 'DATE', 26.167881)
(gynharach, 'cynharach', 'ADJ', , 'DATE', 25.619377)
(heddiw, 'heddiw', 'ADV', , 'DATE', 21.603085)
(., '.', 'PUNCT', , '', 29.340595)
```
Cyfeiriwch at ddogfennaeth swyddogol https://www.spacy.io am gyfarwyddiadau llawn.

Gallwch hefyd ddefnyddio Displacy i ddelweddu'r data, er enghraifft:

```
from spacy import displacy
displacy.serve(doc, style="ent")
```

i weld delweddiad o'r canlyniadau yn eich porwr yn `localhost:5000`

![image](https://user-images.githubusercontent.com/10194573/227287969-6fb2c9dd-b2aa-421f-ab62-ce87078caa10.png)

# Trwydded
Trwyddedir y model o dan drwydded MIT. Mae'r model hwn yn cynnwys gwybodaeth sector cyhoeddus a drwyddedwyd dan Drwydded Llywodraeth Agored v3.0.

----

### English

# A Welsh-language model for spaCy including a tagger, lemmatizer, vectors and NER  

This model includes a pipeline of components that enable the computational processing of Welsh-language texts

## Tokenizer
The tokenizer divides texts into words and other similar units such as numbers and punctuation marks. These units are collectively known as 'tokens'.

```
Dywedodd "Aeth f'annwylyd i'r ffair" heb gyd-destun.
[Dywedodd, ", Aeth, f', annwylyd, i, 'r, ffair, ", heb, gyd-destun, .]
```

## Part of Speech Tagger
The part of speech tagger assigns a specific part of speech tag to each token according to its syntactcic role within the sentences, using for example VERB to tag a verb or PUNCT to tag punctuation marks. The tagset is based on that used by the Universal Dependencies framework.

```
Gwelodd gath heb goler na chynffon.
[('Gwelodd', 'VERB'), ('gath', 'NOUN'), ('heb', 'ADP'), ('goler', 'NOUN'), ('na', 'CONJ'), ('chynffon', 'NOUN'), ('.', 'PUNCT')]
```

## Lemmatizer
The lemmatizer converts the wordfrom into the base form of the word, removing any mutation or verbal or prepositional conjugation.

```
Gwelodd gath heb goler na chynffon.
['gweld', 'cath', 'heb', 'coler', 'na', 'cynffon', '.']
```

It uses the part of speech tags so that it can differentiate between for isntance _ceir_ as a form _cael_ and _ceir_ which is the plural form of _car_ (English: "cars").

```
Ceir rhai garejys yn gwerthu ceir rhad
[('Ceir', 'cael'), ('rhai', 'rhai'), ('garejys', 'garej'), ('yn', 'yn'), ('gwerthu', 'gwerthu'), ('ceir', 'car'), ('rhad', 'rhad')]
```

## Named Entity Recognizer

This component recognizes entities such as person names, place names, and organizations in the sentence.

```
Lansiodd David Hill Jones ei gyfrol newydd The Singularity Show yn nigwyddiad y Cyngor Llyfrau yn Aberystwyth yn gynharach heddiw.
[('Lansiodd', ''), ('David', 'PERSON'), ('Hill', 'PERSON'), ('Jones', 'PERSON'), ('ei', ''), ('gyfrol', ''), ('newydd', ''), ('The', 'WORK_OF_ART'), ('Singularity', 'WORK_OF_ART'), ('Show', 'WORK_OF_ART'), ('yn', ''), ('nigwyddiad', ''), ('y', ''), ('Cyngor', 'ORG'), ('Llyfrau', 'ORG'), ('yn', ''), ('Aberystwyth', 'GPE'), ('yn', 'DATE'), ('gynharach', 'DATE'), ('heddiw', 'DATE'), ('.', '')]
```

These are the entities used:

FAC - Buildings, airports, highways, bridges, etc.  
ORG - Companies, agencies, institutions, etc.  
GPE - Countries, cities, states.  
LOC - Non-GPE locations, mountain ranges, bodies of water.  
PRODUCT - Objects, vehicles, foods, etc. (Not services.)  
EVENT - Named hurricanes, battles, wars, sports events, etc.  
WORK_OF_ART - Titles of books, songs, etc.  
LAW - Named documents made into laws.  
LANGUAGE - Any named language.  
DATE - Absolute or relative dates or periods.  
TIME - Times smaller than a day.  
PERCENT - Percentage, including "%".  
MONEY - Monetary values, including unit.  
QUANTITY - Measurements, as of weight or distance.  
ORDINAL - "first", "second", etc.  
CARDINAL - Numerals that do not fall under another type.

## Word vectors
This model includes word vectors, a numerical representation of the relationship between wordforms. These allow us to use mathematical functions on wordforms, for example to find words that have been used in a similar manner within a large corpus of text.

These are similarity results for 'heddiw':

```
['heddiw', 'heddi', 'ddoe', 'yfory', 'nawr', 'heno', 'heddyw', 'Heddiw', 'rŵan', 'fory']
```

The following code was used:

```
def get_most_similar(query_string, n=10):
    most_similar = nlp.vocab.vectors.most_similar(nlp.vocab[query_string].vector.reshape(1,300), n=n)
    most_similar_wordforms = []
    for key in most_similar[0][0]:
        most_similar_wordforms.append(nlp.vocab[key].orth_)
    return most_similar_wordforms

get_most_similar("heddiw", 10)
```

## Installing the model
Follow the instructions at https://spacy.io/usage er mwyn gosod llyfrgell spaCy.

Download the latest version of the model from https://github.com/techiaith/spacy_cy_tag_lem_ner_lg/releases/latest.

Install the downloaded package using:

`pip install -m enw-ffeil-y-pecyn`

withn your Python environment.

You can confirm that the installation was succesful using:

`python -m spacy info`

At present you will also need to copy the `cy` folder from https://github.com/techiaith/spacy/tree/welsh/spacy/lang to your spaCy installation.

## Usage
To use the model within spaCy:

```
import spacy

nlp = spacy.load("spacy_cy_tag_lem_ner_lg")

doc = nlp("The minister, David Jones the member for South Wales East, spoke on behalf of the Welsh Government today in Llandudno.")
for item in [(t, t.lemma_, t.pos_, t.morph, t.ent_type_, t.vector_norm) for t in doc]:
    print (item)
```

The following output should then be seen:

```
(Lansiodd, 'lansio', 'VERB', , '', 6.1967916)
(David, 'David', 'PROPN', , 'PERSON', 24.811096)
(Hill, 'Hill', 'PROPN', , 'PERSON', 22.11108)
(Jones, 'Jones', 'PROPN', , 'PERSON', 30.422651)
(ei, 'ei', 'DET', , '', 39.050266)
(gyfrol, 'cyfrol', 'NOUN', , '', 29.970655)
(newydd, 'newydd', 'ADJ', , '', 31.02251)
(The, 'The', 'X', , 'WORK_OF_ART', 29.884811)
(Singularity, 'Singularity', 'X', , 'WORK_OF_ART', 0.0)
(Show, 'Show', 'X', , 'WORK_OF_ART', 17.484062)
(yn, 'yn', 'PART', , '', 26.167881)
(nigwyddiad, 'digwyddiad', 'NOUN', , '', 12.935198)
(y, 'y', 'DET', , '', 30.016222)
(Cyngor, 'Cyngor', 'NOUN', , 'ORG', 33.943195)
(Llyfrau, 'Llyfrau', 'NOUN', , 'ORG', 37.831318)
(yn, 'yn', 'ADP', , '', 26.167881)
(Aberystwyth, 'Aberystwyth', 'PROPN', , 'GPE', 25.005033)
(yn, 'yn', 'PART', , 'DATE', 26.167881)
(gynharach, 'cynharach', 'ADJ', , 'DATE', 25.619377)
(heddiw, 'heddiw', 'ADV', , 'DATE', 21.603085)
(., '.', 'PUNCT', , '', 29.340595)
```
Refer to the official documentation at https://www.spacy.io for further instructions.

You can also use Displacy to visualize the data:

```
from spacy import displacy
displacy.serve(doc, style="ent")
```

this will display the parse in your browser at `localhost:5000`

![image](https://user-images.githubusercontent.com/10194573/227287969-6fb2c9dd-b2aa-421f-ab62-ce87078caa10.png)

# Licence
The model is licensed under the MIT licence.  Contains public sector information licensed under the Open Government Licence v3.0.
