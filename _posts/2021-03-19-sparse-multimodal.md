---
layout: post
title: 'Multimodal End-to-End Sparse Model for Emotion Recognition'
date: 2021-03-19 20:30:00
author: "Samuel Cahyawijaya"
description: Existing multimodal solutions incorporate a two-phase pipeline which results in suboptimal performance. We introduces a sparse deep learning model that allows end-to-end learning from raw text, audio, & video altogether with only a single GPU device.
---
<style>

figcaption {
  /* background-color: black;
  color: white; */
  font-style: italic;
  padding: 2px;
  text-align: center;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  /* width: 70%; */
}
/* CSS Simple Pre Code */
pre {
    background: rgba(197, 225, 184, 0.2);
    /* white-space: pre; */
    /* word-wrap: break-word; */
    overflow: auto;
}

pre.code {
    /* margin: 1px 1px; */
    /* border-radius: 2px; */
    /* border: 1px solid #FDF1DD; */
    position: relative;
}

pre.code label {
    /* font-family: sans-serif; */
    /* font-weight: bold; */
    font-size: 13px;
    /* color: #ddd; */
    position: absolute;
    left: 12px;
    top: 9.5px;
    text-align: center;
    width: 20px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    pointer-events: none;
}

pre.code code {
    font-family: "Inconsolata","Monaco","Consolas","Andale Mono","Bitstream Vera Sans Mono","Courier New",Courier,monospace;
    display: block;
    margin: 0 0 0 25px;
    /* padding: 1px 16px 14px; */
    /* border-left: 1px solid #555; */
    overflow-x: auto;
    /* font-size: 13px; */
    /* line-height: 19px; */
    /* color: #ddd; */
}

</style>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

The existing works in multimodal affective computing tasks, such as emotion recognition, generally adopt a two-phase pipeline approach ([Zadeh et al., 2018](https://www.semanticscholar.org/paper/Memory-Fusion-Network-for-Multi-view-Sequential-Zadeh-Liang/609512f19e06bf393cb79fbf57183f75b8d889d2); [Tsai et al., 2018](https://openreview.net/forum?id=rygqqsA9KX), [2019](https://www.semanticscholar.org/paper/Multimodal-Transformer-for-Unaligned-Multimodal-Tsai-Bai/949fef650da4c41afe6049a183b504b3cc91f4bd); [Rahman et al., 2020](https://www.aclweb.org/anthology/2020.acl-main.214/)) which first extracting feature representations for each single modality with hand-crafted algorithms and then performing end-to-end learning with the extracted features. This often leads to complication in designing and choosing the best extraction algorithm and sub-optimal performance of the model because of the feature is very sparse and not tunable. In this blog post, we introduce a sparse model that allows end-to-end learning from raw text, audio, & video altogether with only a single 1080Ti GPU. We compare our proposed method with two different approaches, the first one is the two-phase pipeline model with hand-crafted features and the second one is the fully end-to-end model.

<br />
<br />
<img class="center"  width="55%" src="/assets/img/sparse-mm/highlight.jpg" alt="...">
<figcaption>Figure 1. Illustration of feature extraction from hand-crafted model <b>(left)</b>, fully end-to-end model <b>(middle)</b>, and sparse end-to-end model <b>(right)</b>. The red dots represent the keypoints extracted by hand-crafted models. The areas formed by red lines represent the regions of interest that are processed by (sparse) end-to-end models to extract the features.</figcaption>
<br />
<br />

As shown on the figure above, hand-crafted algorithm such as [OpenFace](https://github.com/TadasBaltrusaitis/OpenFace) and [OpenCV](https://opencv.org/) only capture a small set of keypoints and extract higher level features from the relation between those points. This might produce a fine-grained intuitive representation, although it might not capture all the required points and difficult to be updated, as it needs to be manually tuned. On the contrary, fully end-to-end (FE2E) models enable us to consider all the points in order to extract higher level features. This makes a fully end-to-end model the exact opposite of hand-crafted algorithm, as it might not produce a very intuitive representation but it can capture all the points needed and the model can adjust which point to extract according to the goal of the task. In terms of computation resources, fully end-to-end model generally requires more computation than the hand-crafted counterpart as it takes into account all the points in the image. We try to take both advantage from both methods and invent Multimodal End-to-end Sparse Model (MESM) that not only use less computation than end-to-end model, but also provides an intuitive explaination of what feature is extracted from a given image. We utilize convolution neural network (CNN), [Sparse CNN](https://github.com/facebookresearch/SparseConvNet) and [Transformer](https://papers.nips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf) as the backbone of our model. The architecture of our sparse end-to-end model is shown on the following figure:

<br />
<br />
<img class="center"  width="95%" src="/assets/img/sparse-mm/main_model.jpg" alt="...">
<figcaption>Figure 2. Architecture of our Multimodal End-to-end Sparse Model (MESM). On the left, we show the general architecture flow.  In the middle and on the right, we exhibit the details of the cross-modal sparse CNN block,especially the cross-modal attention layer, which is the key to making the CNN model sparse.</figcaption>
<br />
<br />

In the above figure, we can see that our sparse end-to-end model is divided into 3 main pathways, one for each modality. For text modality, we encode the text with subword tokenization and feed the tokens into a transformer model. For audio and video signal, we use a single layer 2D convolution neural network (CNN) to extract low level features from the image and perform feature extraction with 3 layers of [Cross-modal Sparse CNN block](#cross-modal-sparse-cnn) and then feed the extracted features into a small transformer model. To generate the final prediction, we first standardize the feature dimension for all modalities with a feed-forward network and then we combine all of the features with a multimodal fusion layer, which calculate the linear combination from all the extracted features for each predicted class.

<a name="cross-modal-sparse-cnn"></a>
<br/>
<br/>

## Cross-modal Sparse CNN

<br />
<img class="center"  width="40%" src="/assets/img/sparse-mm/cross-modal-sparse-cnn-block.png" alt="...">
<figcaption>Figure 3. Cross Modal Sparse CNN Block.</figcaption>
<br />

Our cross-modal Sparse CNN block consist of two main components which are a [Cross-modal attention](#cross-modal-attention-layer) layer and a [Sparse CNN](#sparse-cnn-layer) layer. Our cross-model attention layer is used to extract the important point on the audio and video modalities and then pass only the remaining points into sparse CNN for further processing. 

<a name="cross-modal-attention-layer"></a>
<br/>
<br/>

#### Cross-modal attention

<br />
<img class="center" width="20%" src="/assets/img/sparse-mm/cross-modal-attention.png" alt="...">
<figcaption>Figure 4. Cross-modal Attention Layer.</figcaption>
<br />

Our cross-modal attention layer is used to filter out unnecessary from audio and video modalities by calculating attention from the  text modality to the audio and video modalities. We utilize additive attention mechanism ([Bahdanau et al., 2015](https://arxiv.org/pdf/1409.0473.pdf)) over a pair of modalities to compute the attention score. From the resulting attention score, we perform nucleus / top-p sampling with a threshold hyperparameter <b>p</b>. 

We further conduct a deeper analysis to measure the effect of hyperparameter <b>p</b> to the model quality and computational cost required by the model as shown on the following figure

<br />
<img class="center"  width="90%" src="/assets/img/sparse-mm/top-p.jpg" alt="...">

<a name="sparse-cnn-layer"></a>
<br/>
<br />

#### Sparse CNN
Sparse CNN is introduced by ([Graham et al., 2017](https://arxiv.org/abs/1706.01307), [2018](https://arxiv.org/abs/1711.10275)). Sparse CNN is specifically made to avoid unnecessary computation in a sparse tensor that will happen when we use a traditional CNN layer. The following figure shows the different between a traditional CNN layer and a sparse CNN layer. <small>* image is taken from https://github.com/facebookresearch/SparseConvNet</small>

<br />
<img class="center" width="30%" src="https://raw.githubusercontent.com/facebookresearch/SparseConvNet/master/img/i.gif" alt="..."/>
<br />
<img class="center" width="30%" src="https://raw.githubusercontent.com/facebookresearch/SparseConvNet/master/img/img.gif" alt="..."/>
<figcaption>Figure 6. Traditional CNN (top) vs Sparse CNN (bottom)</figcaption>
<br />


## Results & Analysis

<small>* We run our end-to-end experiment in 2 datasets, IEMOCAP & CMU-MOSEI, but as some of the raw data from the provided original dataset has been removed, we reorganize the dataset and generate a new dataset split for both datasets. Please go to [our paper](https://www.semanticscholar.org/paper/Multimodal-End-to-End-Sparse-Model-for-Emotion-Dai-Cahyawijaya/3e90244d0594e36a7c8895eba84cbeba942f3186) for more detail about the dataset reorganization and the setup of our experiment</small>

<br />
<img class="center" width="90%" src="/assets/img/sparse-mm/results.png" alt="..."/>
<br />

From the experimental results and ablation study, we observe that: 
* End-to-end models, both Fully End-to-End (F2E2E) modeland our proposed Multimodal End-to-End Sparse Model (MESM) shows superiority compared to the two-phase pipeline models (LF-LSTM, LF-TRANS, EmoEmbs, and MulT). 
* Our MESM achieves slightly lower results compared to the FE2E model, while requiring less than 60% of the F2E2E model computational cost.
* In two modalities settings, our MESM can achieve a performance that is on par with the FE2Emodel or is even slightly better.

<br />
<img class="center" width="30%" src="/assets/img/sparse-mm/ablation.png" alt="..."/>
<br />

## Case Study
<br />
<img class="center" width="90%" src="/assets/img/sparse-mm/case_study_new.jpg" alt="..."/>
<figcaption>Figure 7. Cross-modal attention to the face region of the image data</figcaption>
<br />

<br />
<img class="center" width="90%" src="/assets/img/sparse-mm/audio_appendix.jpg" alt="..."/>
<figcaption>Figure 8. Cross-modal attention to the mel-spectrum of the audio data. </figcaption>
<br />

For image data, We verify the interpretability of our cross-modal attention by comparing the attention result with the actual Facial Action Coding Systems (FACS) ([Ekman et al., 1997](https://psycnet.apa.org/record/2005-07386-000), [Basori, 2016](http://www.iaescore.com/journals/index.php/IJECE/article/view/1178), [Ahn and Chung, 2017](http://koreascience.or.kr/article/JAKO201710758145067.pdf)) and we can confirm that our attention captures the necessary regions quite well, although it is sometimes fail to capture several features mentioned on the literatures. 

For audio signal, we cannot find any reference on how a sound wave looks like for a specific emotion. In general, at the first layer, the sparse attention capture the area with high spectrum value, meaning there is actually a sounds on that particular frequency, and then start to filter out the features and produce a more sparse feature set from one layer to another.
<br />

## Conclusion
In this work, we first compare and contrast the two-phase pipeline and the fully end-to-end (FE2E) modelling of the multimodal emotion recognition task. Then, we propose our novel multimodal end-to-end sparse model (MESM) to reduce the computational overhead brought by the fully end-to-end model. Additionally, we reorganize two existing datasets to enable fully end-to-end training. The empirical results demonstrate that the FE2E model has an advantage in feature learning and surpasses the current state-of-the-art models that are based on the two-phase pipeline. Furthermore, MESM is able to halve the amount of computation in the feature extraction part compared to FE2E, while maintaining its performance. In our case study, we provide a visualization of the cross-modal attention maps on both visual and acoustic data. It shows that our method can be interpretable, and the cross-modal attention can successfully select important feature points based on different emotion categories. For future work, we believe that incorporating more modalities into the sparse cross-modal attention mechanism is worth exploring since it could potentially enhance the robustness of the sparsity (selection of features). 

## Useful Links
- Github: [https://github.com/wenliangdai/Multimodal-End2end-Sparse](https://github.com/wenliangdai/Multimodal-End2end-Sparse)
- Paper: [https://arxiv.org/pdf/2103.09666.pdf](https://arxiv.org/pdf/2103.09666.pdf)
- Dataset: [https://github.com/wenliangdai/Multimodal-End2end-Sparse](https://github.com/wenliangdai/Multimodal-End2end-Sparse)
