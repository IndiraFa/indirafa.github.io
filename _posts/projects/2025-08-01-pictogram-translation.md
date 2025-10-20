---
layout: page-fullwidth
header:
    image_fullwidth: picto_header_unsplash.jpg
    title: "Parlez-vous picto ?"
subheadline: "NLP"
teaser: This study was conducted in the context of the ToPicto task of ImageCLEF 2025, where our team ranked first. It investigates the performance of a Transformer-based approach for Text-to-Picto and Speech-to-Picto translation from French, using the pre-trained Google-T5 model fine-tuned on the provided dataset.
tags:
    - NLP
    - google-T5
    - transformer
categories:
    - projects
---

📝 **Read the full paper [here](https://ceur-ws.org/Vol-4038/paper_192.pdf)**

## Context

Augmentative and Alternative Communication (AAC) encompasses methods used to supplement or
replace speech and writing, particularly for individuals with speech and language impairments. Existing
AAC systems are diverse, with many implementations utilizing pictograms. This visual and symbol-
based approach can significantly enhance communication effectiveness and has been shown to be
efficient [^1]. A key challenge is bridging the gap between AAC users and the broader society. Thus,
developing tools that convert speech and text modalities into a sequence of pictograms is essential
for facilitating effective communication between these two groups. Recent advances in Transformer
models in natural language processing tasks, along with the results from the previous ToPicto Challenge
2024 [^2], led us to further explore these architectures for Text-to-Picto and Speech-to-Text-to-Picto
translation tasks. This research focuses on using the pre-trained Google T5 model and fine-tuning it
with the new corpus provided for the **ToPicto task of ImageCLEF 2025** [^3], [^4].

[^1]: C. Syriopoulou-Delli, E. Gkiolnta, Effectiveness of different types of augmentative and alternativecommunication (aac) in improving communication skills and in enhancing the vocabulary of children with asd: a review, Review Journal of Autism and Developmental Disorders 9 (2021). doi:10.1007/s40489-021-00269-4.

[^2]: B. Ionescu, H. Müller, A. Drăgulinescu, J. Rückert, A. Ben Abacha, A. García Seco de Herrera, L. Bloch, R. Brüngel, A. Idrissi-Yaghir, H. Schäfer, C. S. Schmidt, T. M. Pakull, H. Damm, B. Bracke, C. M. Friedrich, A. Andrei, Y. Prokopchuk, D. Karpenka, A. Radzhabov, V. Kovalev, C. Macaire, D. Schwab, B. Lecouteux, E. Esperança-Rodier, W. Yim, Y. Fu, Z. Sun, M. Yetisgen, F. Xia, S. A. Hicks, M. A. Riegler, V. Thambawita, A. Storås, P. Halvorsen, M. Heinrich, J. Kiesel, M. Potthast, B. Stein, Overview of imageclef 2024: Multimedia retrieval in medical applications, in: Experimental IR Meets Multilinguality, Multimodality, and Interaction, Proceedings of the 15th International Conference of the CLEF Association (CLEF 2024), Lecture Notes in Computer Science, Springer, Grenoble, France, 2024.

[^3]: C. Macaire, D. Fabre, B. Lecouteux, D. Schwab, Overview of the 2025 imagecleftopicto task – investigating the generation of pictogram sequences from text and speech, in: CLEF2025 Working Notes, CEUR Workshop Proceedings, CEUR-WS.org, Madrid, Spain, 2025.

[^4]: B. Ionescu, H. Müller, D.-C. Stanciu, A.-G. Andrei, A. Radzhabov, Y. Prokopchuk, Ştefan, Liviu- Daniel, M.-G. Constantin, M. Dogariu, V. Kovalev, H. Damm, J. Rückert, A. Ben Abacha, A. García Seco de Herrera, C. M. Friedrich, L. Bloch, R. Brüngel, A. Idrissi-Yaghir, H. Schäfer, C. S. Schmidt, T. M. G. Pakull, B. Bracke, O. Pelka, B. Eryilmaz, H. Becker, W.-W. Yim, N. Codella, R. A. Novoa, J. Malvehy, D. Dimitrov, R. J. Das, Z. Xie, H. M. Shan, P. Nakov, I. Koychev, S. A. Hicks, S. Gautam, M. A. Riegler, V. Thambawita, P. Halvorsen, D. Fabre, C. Macaire, B. Lecouteux, D. Schwab, M. Potthast, M. Heinrich, J. Kiesel, M. Wolter, B. Stein, Overview of imageclef 2025: Multimedia retrieval in medical, social media and content recommendation applications, in: Experimental IR Meets Multilinguality, Multimodality, and Interaction, Proceedings of the 16th International Conference of the CLEF Association (CLEF 2025), Springer Lecture Notes in Computer Science LNCS, Madrid, Spain, 2025.


![Example](/images/picto_case2_small.png)


## Dataset

The dataset used in this study is sourced from the **CommonVoice v.15 corpus** [^5] and the **Orféo corpus** [^6]. CommonVoice is a multilingual, publicly available voice dataset recorded by users on the
[Common Voice platform](http://voice.mozilla.org/). It is intended for speech technology research and
development and is based on text from various public domain sources. Only French language data
were used from this dataset. Orféo is a corpus consisting of both spoken and written French samples.
It contains interactions between adults, adults and children, as well as between children. It has the
advantage of being representative of the interactions observed between caregivers and individuals who
rely on pictograms due to language impairments. 

Training, validation, and test splits consist of 20,177,
1,208, and 2,901 utterances, respectively. For the Speech-to-Picto task, a corresponding audio sequence
associated with a pictogram sequence is provided. For the Text-to-Picto translation
task, a corresponding sequence of terms associated with a pictogram sequence is provided, derived
from the speech transcription.


ARASAAC pictograms are used as a reference for pictogram translation. Images can be obtained via
the ARASAAC API using ````https://api.arasaac.org/v1/pictograms/{pictogram_ref_number}````

[^5]:R. Ardila, M. Branson, K. Davis, M. Kohler, J. Meyer, M. Henretty, R. Morais, L. Saunders, F. Tyers, G. Weber, Common voice: A massively-multilingual speech corpus, Proceedings of the Twelfth Language Resources and Evaluation Conference (2020) 4218–4222. URL: <https://aclanthology.org/2020.lrec-1.520/>

[^6]:C. Benzitoun, J.-M. Debaisieux, H.-J. Deulofeu, Le projet ORFÉO : un corpus d’étude pour le français contemporain, Corpus (2016). URL: http://journals.openedition.org/corpus/2936. doi:10.4000/corpus.2936, en ligne, mis en ligne le 15 janvier 2017, consulté le 24 mai 2025.

## Approach

### Text-to-Picto

The Text-to-Picto task was addressed using a text-to-text approach based on the
pre-trained Google T5 model, which is fine-tuned on the provided corpus. The output sequence of French
terms must correspond to a sequence of French pictogram terms and comply with the specifications of
AAC. 

T5 is an encoder-decoder Transformer available in various sizes, ranging from 60 million to 11
billion parameters [^7]. Its ability to handle a wide range of NLP tasks by treating them all as text-to-text
problems makes it an attractive choice for Text-to-Picto translation. 

[^7]: C. Raffel, N. Shazeer, A. Roberts, K. Lee, S. Narang, M. Matena, Y. Zhou, W. Li, P. J. Liu, Exploring the limits of transfer learning with a unified text-to-text transformer, 2023. URL: https://arxiv.org/abs/1910.10683. arXiv:1910.10683

### Speech-to-Picto
For the Speech-to-Picto task, the speech is first converted to text using two models of the Whisper
family4 [^8] before applying the same Text-to-Picto approach described above. 

The models used are
Whisper-small (244 million parameters) and Whisper-large (1,550 million parameters). We directly use
the Whisper models for inference; hence, no model training is involved in this process. The choice to
implement a cascade approach (Speech-to-Text followed by Text-to-Picto) rather than directly fine-
tuning on the audio was primarily dictated by the limited time available for the project.

[^8]: A. Radford, J. W. Kim, T. Xu, G. Brockman, C. McLeavey, I. Sutskever, Robust speech recognition via large-scale weak supervision, 2022. URL: https://arxiv.org/abs/2212.04356. doi:10.48550/ARXIV.2212.04356.

## Results

The model performance is evaluated using three different metrics by comparing the predicted
pictogram sequence to the target: **SacreBLEU, METEOR, PictoER.**

Using the T5-large version of the model gave the best performance during training, yielding scores of 93.0, 95.7, and 3.4 for SacreBLEU, METEOR, and
PictoER, respectively. However, the scores obtained on the validation and test sets are substantially
lower, indicating limited generalization to unseen data. 

Some representative examples are selected for qualitative analysis to highlight behavioral differences
between model sizes and inherent challenges associated with text-to-pictogram translation for this
particular dataset. The pictogram sequences are generated from predicted tokens using the Hugging
Face platform5. 

![Text example](/images/case1_text.png)

*Exemple - Reference, target and sequences obtained with T5-small, -base and -large on the utterance of id cefc-valibel-accFJ1r-16 (deviations from target are shown in bold, invalid pictograms are underlined).*

![Picto example](/images/case1_picto.png)

*Example - Pictogram sequences associated with target (framed in black) and sequences obtained with T5-small, -base and -large on the utterance of id cefc-valibel-accFJ1r-16 (invalid pictograms are left blank).*



Key improvements observed with the T5-large model, compared to smaller versions, include a reduced
tendency to generate words that do not correspond to any existing pictogram. Both T5-base and T5-
large demonstrate enhanced ability to correctly translate past tense and proper nouns of names and
places, which are often translated by a generic pictogram in the target. Moreover, sentences containing
numbers are challenging for smaller models but are translated more accurately by T5-large.

Furthermore, we investigate the models’ ability to adapt to the in-domain training vocabulary,
specifically the pictogram terms encountered during training. By "training vocabulary," we refer to
the unique words present in all sentences within the training set. We differentiate between the source
vocabulary (words) and the target vocabulary (pictogram terms). For instance, the training set contains
20,177 sentences with 23,731 words in the source vocabulary and 4,354 pictogram terms in the target
vocabulary. Similarly, the validation set includes 1,208 sentences with 3,558 words in the source
vocabulary and 1,502 pictogram terms in the target vocabulary. Notably, the target vocabularies for the
training and validation sets share 1,432 pictogram terms.

The T5 models are pre-trained on a vast amount of diverse text data; therefore, we expect the models
to incorporate words seen during their pre-training in their predictions. During fine-tuning, the models
should learn a new vocabulary of pictogram terms. We estimate the model’s ability to do so by counting
the words in the mutual vocabulary between the target sentences and the model predictions. We
find that the T5-small and T5-base models include between 2,700 and 2,800 of the pictogram terms
encountered during training in their predictions. In comparison, the T5-large model appears to better
adopt the target vocabulary seen during training, with approximately 3,500 mutual pictogram terms.
This suggests that T5-large has a greater capacity to learn and utilize the target vocabulary effectively.

To solve the Speech-to-Picto task, we combine a pre-trained ASR model with the best model fine-tuned
for Text-to-Picto translation. No training is involved in this process. Instead, we directly use the Whisper
models for inference, hence, we do not make use of the training and validation sets for this task. Two different models from the Whisper family are used to produce transcripts from
the audio of the test data. As expected, the larger Whisper model outperforms the smaller one, most
likely due to higher-quality transcriptions.

## Conclusion

In conclusion, the fine-tuned Google T5-large model exhibits strong performance in translating French
text into appropriate sequences of pictograms. These promising results contribute to efforts to bridge the
gap between AAC users and the broader society, facilitating effective communication. However, there
is still room to improve the model’s ability to generalize to unseen data and to reduce the generation of
non-pictogram words.

Additionally, the Speech-to-Text-to-Picto solution, which utilizes Whisper to produce transcripts
and the fine-tuned T5 model for translation, shows potential. Further refinement is needed to ensure
accurate translations from spoken language to pictogram sequences.
In this study, the maximum number of tokens generated by the model was set to 64, since the longest
sentences in the test set contained 62 words. Increasing this parameter could potentially improve
predictions, depending on how tokens are generated with the T5 tokenizer. 

To enhance generalization
on unseen data, techniques such as regularization or dropout could be employed, or the model could be
trained on more diverse datasets. Furthermore, models fine-tuned for Text-to-Picto translation must
adapt to a specialized vocabulary of pictogram terms. Future work could focus on further investigation
and optimization of this in-domain adaptation.