# neuralcoref_model_training_2020
This is a detailed step by step walkthrough of how to replicate the coreference solution training of OntoNotes 5.0 Release dataset using the model and information provided by https://github.com/huggingface/neuralcoref.

# purpose
Neuralcoref is a great coreference model written in Python, but because of the rapid development of Python and relevant packages, there might occur some incompability issue if strictly follow the provided training instructions, which could be found here: https://github.com/huggingface/neuralcoref/blob/master/neuralcoref/train/training.md. Also, the preparation of the dataset (specifically the one used in the training instruction: OntoNotes 5.0 Release) is not an easy process. Therefore, I want to share my entire process in detail to help others who potentially would use this model.

# Conll-formatted-OntoNote-Data
## "Conll-formatted-OntoNote-Data"
In the folder "Conll-formatted-OntoNote-Data", there are conll-formatted train & test& development datasets of OntoNotes 5.0 Release. The file types include ".v4_gold_conll", ".v4_auto_conll", ".v4_auto_prop", ".v4_auto_sense", and ".v4_auto_skel". The spliting method is from http://conll.cemantix.org/2012/data.html, and can be done by following the "Get the data" part in https://github.com/huggingface/neuralcoref/blob/master/neuralcoref/train/training.md#get-the-data.

Note that Python 2 is needed during this process ("Get the data"). You can switch between Python2 and Python3 following the method provided in https://docs.anaconda.com/anaconda/user-guide/tasks/switch-environment/:

I'm using Mac so this is how I do:
### Python3 -> Python2
conda create --name py2 python=2.7

conda activate py2

## "Conll-formatted-OntoNote-Data-v4_gold_conll"
In the folder "Conll-formatted-OntoNote-Data-v4_gold_conll", there are conll-formatted train & test& development datasets of OntoNotes 5.0 Release with only those whose file type is ".v4_gold_conll". This process can be done by following the "Prepare the data" part in https://github.com/huggingface/neuralcoref/blob/master/neuralcoref/train/training.md#prepare-the-data.

Note that Python 3 is needed during this process ("Get the data"). You can switch between Python2 and Python3 following the method provided in https://docs.anaconda.com/anaconda/user-guide/tasks/switch-environment/:

I'm using Mac so this is how I do:
### Python2 -> Python3
conda create --name py3 python=3.5

conda activate py3

Another thing worth noting is that, there are several printing statements in the "conllparser.py" that cause syntax error when I run it. So I fix it by comment them out. Those are just print statements so won't cause a big chance. The following are the specific lines I commented out:
1. Line 1930: print(f"🌋 Extracting mentions features with {self.n_jobs} job(s)")
2. Line 1933: print(f"🌋 Building and gathering array with {self.n_jobs} job(s)")
3. Line 1992: print(f"voc saved in {path}, length: {len(vocabulary)}")


