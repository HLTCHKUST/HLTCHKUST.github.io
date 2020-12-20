---
layout: page
title: Adapter-Bot
description: Adapter-Bot provides on-demend dialogue skills (e.g., emphatic response, weather information, movie recommendation) via different adapters.
img: /assets/img/adaptorbot.png
importance: 3
---
Original Paper: <a href="https://arxiv.org/pdf/2008.12579.pdf" target="_blank"><i class="far fa-fw fa-file"></i></a>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/adaptorbot.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    The Adapter-Bot: All-In-One Controllable Conversational Model
</div>

Considerable progress has been made towards conversational models that generate coherent and fluent responses by training large language models on large dialogue datasets. These models have little or no control of the generated responses and miss two important features: continuous dialogue skills integration and seamlessly leveraging diverse knowledge sources. In this paper, we propose the Adapter-Bot, a dialogue model that uses a fixed backbone conversational model such as DialGPT (Zhang et al., 2019) and triggers on-demand dialogue skills (e.g., emphatic response, weather information, movie recommendation) via different adapters (Houlsby et al., 2019). Each adapter can be trained independently, thus allowing a continual integration of skills without retraining the entire model. Depending on the skills, the model is able to process multiple knowledge types, such as text, tables, and graphs, in a seamless manner. The dialogue skills can be triggered automatically via a dialogue manager, or manually, thus allowing high-level control of the generated responses. At the current stage, we have implemented 12 response styles (e.g., positive, negative etc.), 8 goal-oriented skills (e.g. weather information, movie recommendation, etc.), and personalized and emphatic responses. We evaluate our model using automatic evaluation by comparing it with existing state-of-the-art conversational models, and we have released an interactive system at <a href="https://adapterbot.emos.ai/" target="_blank">here</a>

Demo video at <a href="https://youtu.be/Jz8KWE_gKH0" target="_blank">here</a>.
