# neuralcoref_model_training_2020
This is a detailed step by step walkthrough of how to replicate the coreference solution training of OntoNotes 5.0 Release dataset using the model and information provided by https://github.com/huggingface/neuralcoref.

# purpose
Neuralcoref is a great coreference model written in Python, but because of the rapid development of Python and relevant packages, there might occur some incompability issue if strictly follow the provided training instructions, which could be found here: https://github.com/huggingface/neuralcoref/blob/master/neuralcoref/train/training.md. Also, the preparation of the dataset (specifically the one used in the training instruction: OntoNotes 5.0 Release) is not an easy process. Therefore, I want to share my entire process in detail to help others who potentially would use this model.

# File description
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

# Step description

### 1.Download OntoNote dataset
Download OntoNote dataset here: https://catalog.ldc.upenn.edu/LDC2013T19.
Note that if you don't have an account, you need to first register as a member of an organization to obtain access to the dataset. This is how you can register:
- Open sign up link here: https://catalog.ldc.upenn.edu/signup. 
- In "New User Registration" page, "Organization" is originally "Guest". However, guest status does not provide data access. You have to choose your own organization, or you create one yourself by clicking the "create your organization".
- After being accepted into your organization (this might take a few days, you can email "ldc@ldc.upenn.edu" to ask for your own organization administrator's contact information and contact by yourself to accelerate the process), open https://catalog.ldc.upenn.edu/LDC2013T19 and scroll to the bottom of the page, then click request.
- After successfully order the dataset, dataset should be here https://catalog.ldc.upenn.edu/organization/downloads after one or two days.

### 2.Package installation
- spaCy:
Make sure that the spaCy version is younger than 2.1.0, otherwise it might have a Value Error "ValueError: spacy.strings.StringStore size changed, may indicate binary incompatibility. Expected 112 from C header, got 88 from PyObject".
conda install -c conda-forge spacy
- Pytorch
conda install pytorch -c pytorch
- Tensorflow

### 3. Follow the "Get Data" part 
https://github.com/huggingface/neuralcoref/blob/master/neuralcoref/train/training.md#get-the-data

Note that Python 2 is needed during this process ("Get the data"). You can switch between Python2 and Python3 following the method provided in https://docs.anaconda.com/anaconda/user-guide/tasks/switch-environment/:

I'm using Mac so this is how I do:
### Python3 -> Python2
conda create --name py2 python=2.7

conda activate py2

### 4. Prepare data
- clone the files in https://github.com/huggingface/neuralcoref/tree/master/neuralcoref/train to your own directory
- Eliminate all "neuralcoref.train." prefix in the imports of all files
For instance: In conllparser.py, there are two imports contains "neuralcoref.train.", modify in this way:
1. from neuralcoref.train.compat import unicode_  ----> from compat import unicode_
2. from neuralcoref.train.utils import encode_distance, parallel_process  ----> from utils import encode_distance, parallel_process

- After reformat the import code, execute the following code:
python -m conllparser --path ./$path_to_data_directory/train/
python -m conllparser --path ./$path_to_data_directory/test/
python -m conllparser --path ./$path_to_data_directory/dev/

### 5. Train data
Execute the following code after making sure that all "neuralcoref.train." prefix in the imports of all files are eliminated:
python -m learn --train ./data/train/ --eval ./data/dev/

test_corefs.txt and test_mentions.txt would be created in your directory.

