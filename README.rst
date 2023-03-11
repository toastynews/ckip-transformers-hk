CKIP Transformers HK
-----------------

`CKIP Transformers <https://github.com/ckiplab/ckip-transformers>`__ is a set of models for Chinese NLP processing created by `CKIP Lab <https://ckip.iis.sinica.edu.tw>`__. 
This repo contains information about a compatible word segmentation model for Hongkongese/Cantonese. It can be loaded and used directly by CKIP Transformers. The only difference between the original repo and this repo is this README file.

Models
------

There are two versions of this model.
* HK - trained on only Hong Kong text (HKCanCor and CityU)
* HKT - trained on a combination of Hong Kong and Taiwan text (HKCanCor, CityU and AS)

The HK version is slightly better for Hongkongese, while the HKT version is slightly worse for Hongkongese but a lot better for Standard Chinese text.
Base and Small sizs are provided for each version. The different in performance is minor so it is recommended to use the Small size unless you found something wrong with it.
Different from CKIP Transformers, the base model is ELECTRA and not BERT, see `electra-hongkongese <https://github.com/toastynews/electra-hongkongese>`__. It uses a custom vocabulary that is more suited for Hong Kong text.

NLP Task Models
* `ELECTRA HK Small — Word Segmentation <https://huggingface.co/toastynews/electra-hongkongese-small-hk-ws>`_: ``toastynews/electra-hongkongese-small-hk-ws``
* `ELECTRA HKT Small — Word Segmentation <https://huggingface.co/toastynews/electra-hongkongese-small-hkt-ws>`_: ``toastynews/electra-hongkongese-small-hkt-ws``
* `ELECTRA HK Base — Word Segmentation <https://huggingface.co/toastynews/electra-hongkongese-base-hk-ws>`_: ``toastynews/electra-hongkongese-base-hk-ws``   
* `ELECTRA HKT Base — Word Segmentation <https://huggingface.co/toastynews/electra-hongkongese-base-hkt-ws>`_: ``toastynews/electra-hongkongese-base-hkt-ws``   

Model Usage
^^^^^^^^^^^

See the `CKIP Transformers <https://github.com/ckiplab/ckip-transformers#model-usage>`__ repo for model usage instructions. The only difference is that it does not use the ``bert-base-chinese`` tokenizer. Simply use ``AutoTokenizer`` from the model.

Model Fine-Tunning
^^^^^^^^^^^^^^^^^^

Instructions for reproducing this model and code to generate training files is in `finetune-ckip-transformers <https://github.com/toastynews/finetune-ckip-transformers>`__.

Training Corpus
^^^^^^^^^^^^^^^

The WS task is trained on the following datasets.
* `HKCanCor <https://pycantonese.org/data.html#built-in-data>`__ - Hong Kong Cantonese Corpus from Nanyang Technological University.
* `CityU <http://sighan.cs.uchicago.edu/bakeoff2005/>`__ - Hong Kong news text from City University of Hong Kong. Only the training set is used.
* `AS <http://sighan.cs.uchicago.edu/bakeoff2005/>`__ - Variety of Taiwan Chinese text from Academia Sinica. Only the training set is used.

NLP Tools
---------

See the `CKIP Transformers <https://github.com/ckiplab/ckip-transformers#nlp-tools>`__ repo for full instructions. 
The model can be specified by ``model_name`` and it will be downloaded automatically.

NLP Tools Usage (abridged)
^^^^^^^^^^^^^^^^^^^^^^^^^^

The following are the abridged instructions illustrating how to use this model.

1. Import module
""""""""""""""""

.. code-block:: python

   from ckip_transformers.nlp import CkipWordSegmenter

2. Load models
""""""""""""""

| We currently only support the word segmenter from the NLP tools.

.. code-block:: python

   # Initialize drivers
   ws_driver  = CkipWordSegmenter(model_name="toastynews/electra-hongkongese-base-hkt-ws")

3. Run pipeline
"""""""""""""""

| The input for word segmentation must be a list of sentences.

.. code-block:: python

   # Input text
   text = [
      "威院ICU顧問醫生Tom Buckley作供時批評",
      "兒子生性病母倍感安慰，獅子山下體現香港精神",
   ]

   # Run pipeline
   ws  = ws_driver(text)

4. Show results
"""""""""""""""

.. code-block:: python

   print(' '.join(ws[0]))
   print(' '.join(ws[1]))

.. code-block:: text   

  威院 ICU 顧問 醫生 Tom  Buckley 作供 時 批評
  兒子 生 性 病母 倍感 安慰 ， 獅子山 下 體現 香港 精神

NLP Tools Performance
^^^^^^^^^^^^^^^^^^^^^

The following is a performance comparison between this model and the original model.
* UD yue_hk - the `yue_hk <https://universaldependencies.org/treebanks/yue_hk/index.html>`__ dataset from Universal Dependencies.
* UD zh_hk - the `zh_hk <https://universaldependencies.org/treebanks/zh_hk/index.html>`__ dataset from Universal Dependencies.
* HKCanCor - the same HKCanCor data that this model was trained on. It is only reported for completeness.
* CityU - the test set from the same CityU corpus.
* AS - the test set from the same AS corpus.

Word Segmentation Performance (F1)
""""""""""""""""""""""""""""""""""
========================  ===========  ===========  ===========  ===========  ===========
Tool                       UD yue_hk    UD zh_hk      HKCanCor      CityU         AS
========================  ===========  ===========  ===========  ===========  ===========
CKIP BERT Base              89.41%       92.70%       83.81%        91.95%     **98.06%**     
CKIP ELECTRA HK Base        94.62%     **93.30%**   **98.95%**    **98.06%**     92.25%            
CKIP ELECTRA HKT Base       94.04%       93.27%       98.75%        97.66%       96.52%            
CKIP BERT Tiny              85.02%       92.07%       78.18%        89.93%       97.87%          
CKIP ELECTRA HK Small     **94.68%**     92.77%       97.69%        97.50%       91.87%          
CKIP ELECTRA HKT Small      93.89%       93.14%       98.07%        97.12%       96.44%          
========================  ===========  ===========  ===========  ===========  ===========

License
-------

|GPL-3.0|

Copyright (c) 2021 `CKIP Lab <https://ckip.iis.sinica.edu.tw>`__ under the `GPL-3.0 License <https://www.gnu.org/licenses/gpl-3.0.html>`__.

.. |GPL-3.0| image:: https://www.gnu.org/graphics/gplv3-with-text-136x68.png
   :target: https://www.gnu.org/licenses/gpl-3.0.html
