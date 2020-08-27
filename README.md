# Stockchatbot
This software package is a chatbot that can get real-time stock information. Its framework is based on `Rasa_NLU` and its pipline is `"spacy_sklearn"`. With pre-defined entities and machine learning, this chatbot can `classify intents` and `extract named entities` from natural language concerning stock information query. With `slot filling` and `finite state machine`, this chatbot can realize natural language dialogue.<br>
The code of this chatbot can be seen in the `stockchatbot.ipynb`<br>
## Catalog
* Video Demo
* Configuration Guide
   * Rasa NLU
   * iexfinance
   * wxpy
   * Matplotlib
* Usage Guide
   * Create NLU dataset and entity dataset
   * Define NLU model configuration
   * Train NLU model
   * Call API of iexfinance & wxpy
   * Try it out
* Authors
## Video Demo
This gif demo shows what the Stockchatbot can do.
.<div align=center><img src="https://github.com/Tknight01/Stockchatbot/blob/master/demogif.gif" /></div>
## Installation Guide
### Rasa NLU
We take [Rasa NLU](https://www.rasa.com/) as the framework of natural language chatbot. As a result, Rasa NLU needs to be installed and some modules of Rasa NLU need to be imported.<br>
Install the package of Rasa NLU.<br>
```
## pip install rasa_nlu
```
If you want to use the bleeding edge version of Rasa NLU, you can get it from [github](https://github.com/RasaHQ/rasa_nlu).<br>
```
## git clone https://github.com/RasaHQ/rasa_nlu.git
## cd rasa_nlu
## pip install -r requirements.txt
## pip install -e .
```
Import modules of Rasa NLU. These modules help us configure and train the model.<br>
```
## from rasa_nlu.training_data import load_data
## from rasa_nlu.config import RasaNLUModelConfig
## from rasa_nlu.model import Trainer
## from rasa_nlu import config
```
### iexfinance
[iexfinance](https://pypi.org/project/iexfinance/0.3.1/) is an easy-to-use toolkit to obtain data for stocks:<br>
* Real-time and delayed quotes
* Historical data
* Financial statements
* Analyst estimates, Price targets
* Corporate actions
* ...

Installation from [PyPI](https://pypi.org/project/iexfinance/0.3.1/) with pip:<br>
```
## pip3 install iexfinance
```
Installation from [development repository](https://github.com/addisonlynch/iexfinance):<br>
```
## git clone https://github.com/addisonlynch/iexfinance.git
## cd iexfinance
## python3 setup.py install
```
### wxpy
[wxpy](https://github.com/youfou/wxpy) is an API of social software WeChat, it could be used as an automatic send/receive tool of our chatbot.<br>
Installation from [github](https://github.com/youfou/wxpy):<br>
```
## pip install -U wxpy
```
### Matplotlib
[Matplotlib](https://matplotlib.org/) is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. You can generate plots, histograms, power spectra, bar charts, errorcharts, scatterplots, etc., with just a few lines of code.<br>
Installation from [Matplotlib](https://matplotlib.org/):
```
## pip install -U matplotlib
```
We also need some modules of Matplotlib to draw the figure concerning stock information.
```
## import matplotlib as mpl
## import matplotlib.pyplot as plt
## import mpl_finance
## import mpl_finance as mpf
## from matplotlib.pylab import date2num
```
## Configuration and Usage Guide
### Create NLU dataset and entity dataset
The first thing to use this framework is to define user's messages that this chatbot can understand. You need to define intents and provide different expression of the same intent.This dataset should be in the form of `JSON`. Like these:<br>
```
"train_data_demo": {
    "common_examples": [
      {
          "text": "I want to get current price of tesla",
          "intent": "current_price",
          "entities": [
            {
              "start":31,
              "end":36,
              "value":"TSLA",
              "entity":"company"
            }
          ]
        },
      {
        "text": "a developmental share in the technology sector",
        "intent": "stock_search",
        "entities": [
          {
            "start": 2,
            "end": 15,
            "value": "developmental",
            "entity": "share"
          },
          {
            "start": 29,
            "end": 39,
            "value": "technology",
            "entity": "sector"
          }
        ]
      }
    ]
  } 
```
In order to achieve the function of searching for some specific stocks, you also need to build a `SQLite` dataset to hold these stock information. In this project, we create a table in the `SQLite` dataset and saved it as `stock.db`<br>
### Define NLU model configuration
In this project, due to examples amount in the corpus of stock information, we take the "spacy_sklearn" pipline to configure our model. What's more, you need to pre-define the dataset of intents and entities. There are alternative choices provided by [Rasa NLU](https://rasa.com/docs/nlu/choosing_pipeline/).<br>
The configurations can be seen as:<br>
```
language: "en"

pipline:
- name: "nlp_spacy"
- name: "tokenizer_spacy"
- name: "intent_entity_featurizer_regex"
- name: "intent_featurizer_spacy"
- name: "ner_crf"
- name: "ner_synonyms"
- name: "intent_classifier_sklearn"
```
spaCy can be installed as:<br>
```
## pip install rasa_nlu[spacy]
## python -m spacy download en_core_web_md
## python -m spacy link en_core_web_md en
```
In this configuration, Intents are classified by `a linear SVM` which gets optimized using a grid search. Named entity recognition and extraction are achieved by the `conditional random field model`. CRFs can be thought of as an undirected Markov chain where the time steps are words and the states are entity classes. Features of the words give probabilities to certain entity classes, as are transitions between neighbouring entity tags: the most likely set of tags is then calculated and returned. In addition, `regular expression` is also be used into intent classifier / entity extractor to simplify classification.<br> 
### Train NLU model
To train the model, start the `rasa_nlu.train` command, and tell it where to find your configuration and your training data. Then, you need to create a interpreter to classify intent and extract entity.<br>
```
## trainer = Trainer(config.load("config_spacy.yml"))
## training_data = load_data('train_data_demo.json')
## interpreter = trainer.train(training_data)
## interpreter.parse(message)['intent']['name']
## interpreter.parse(message)['entities']['value']
```  
### Call API of iexfinance & wxpy
The [iex-examples](https://github.com/addisonlynch/iex-examples) repository provides a number of detailed examples of iexfinance usage. We just show one of these:<br>
To obtain real-time quotes for one or more symbols, use the `get_price` method of the `Stock` object:<br>
```
## from iexfinance.stocks import Stock
## tsla = Stock('TSLA')
## tsla.get_price()
```
In order to make our chatbot more attractive, we deploy it into WeChat by [wxpy](https://github.com/youfou/wxpy).<br>
First, you need to import `wxpy` and initialize a Bot():<br>
```
## from wxpy import *
## bot = Bot()
```
Then find my_friend and define a auto reply function:<br>
```
## my_friend = bot.friends().search('name', sex='', city='')[0]
## @bot.register(my_friend)
## def reply_my_friend(msg):
##    return 'received: {} ({})'.format(msg.text, msg.type)
```
At last, integrate these programs into chatbot framework.<br>
### Try it out
These are some figures produced in our chatbot, you can find these functions in `stockchatbot.ipynb`.<br>
```
## get_monthly_volume_price("BA")
```
![monthvolumeprice](https://github.com/Tknight01/Stockchatbot/blob/master/monthvolumeprice.png)
```
## get_sector_perform()
```
![sectorperformance](https://github.com/Tknight01/Stockchatbot/blob/master/sectorperformance.png)
```
## get_monthly_abstract_price("AIR")
```
![monthabstractprice](https://github.com/Tknight01/Stockchatbot/blob/master/monthabstractprice.png)
The final result can be seen in the `Stockchatbot video demo.mp4`.<br>
## Authors
This program was primarily written by Ke Xu at USTC who was supervised and advised by [Ph.D Fan Zhang](http://www.mit.edu/~f_zhang/) at IBM & MIT.
## Contact Information
Email: xukemse@mail.ustc.edu.cn
