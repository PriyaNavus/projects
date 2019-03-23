# Stockchatbot
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
.<div align=center><img src="https://github.com/Tknight01/Ke/blob/master/Stockchatbot%20gif%20demo.gif" /></div>
## Configuration and Installation Guide
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
* ...<br>
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
### Matplotlib
## Usage Guide
### Create NLU dataset and entity dataset
### Define NLU model configuration
### Train NLU model
### Call API of iexfinance & wxpy
### Try it out
## Authors

