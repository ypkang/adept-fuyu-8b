---
license: cc
---
# Fuyu-8B Model Card

## Model

[Fuyu-8B](https://www.adept.ai/blog/fuyu-8b) is a multi-modal text and image transformer trained by [Adept AI](https://www.adept.ai/).

Architecturally, Fuyu is a vanilla decoder-only transformer - there is no image encoder. 
Image patches are instead linearly projected into the first layer of the transformer, bypassing the embedding lookup. 
We simply treat the transformer decoder like an image transformer (albeit with no pooling and causal attention).
See the below diagram for more details.

![architecture](architecture.png)

This simplification allows us to support arbitrary image resolutions. 
To accomplish this, we treat the sequence of image tokens like the sequence of text tokens. 
We remove image-specific position embeddings and feed in as many image tokens as necessary in raster-scan order. 
To tell the model when a line has broken, we simply use a special image-newline character. 
The model can use its existing position embeddings to reason about different image sizes, and we can use images of arbitrary size at training time, removing the need for separate high and low-resolution training stages.

### Model Description

- **Developed by:** Adept-AI
- **Model type:** Decoder-only multi-modal transformer model 
- **License:** [CC-BY-NC](https://creativecommons.org/licenses/by-nc/4.0/deed.en)
- **Model Description:** This is a multi-modal model that can consume images and text and produce test. 
- **Resources for more information:** Check out our [blog post](https://www.adept.ai/blog/fuyu-8b).

## Evaluation
Though not the focus of this model, we did evaluate it on standard image understanding benchmarks:

| Eval Task           | Fuyu-8B | Fuyu-Medium       | LLaVA 1.5 (13.5B) | QWEN-VL (10B) | PALI-X (55B) | PALM-e-12B | PALM-e-562B |
| ------------------- | ------- | ----------------- | ----------------- | ------------- | ------------ | ---------- | ----------- |
| VQAv2               | 74.2    |     77.4          | 80                | 79.5          | 86.1         | 76.2       | 80.0        |
| OKVQA               | 60.6    |     63.1          | n/a               | 58.6          | 66.1         | 55.5       | 66.1        |
| COCO Captions       | 141     |     138           | n/a               | n/a           | 149          | 135        | 138         |
| AI2D                | 64.5    |     73.7          | n/a               | 62.3          | 81.2         | n/a        | n/a         |

## Uses

### Direct Use

The model is intended for research purposes only. 
**Because this is a raw model release, we have not added further finetuning, postprocessing or sampling strategies to control for undesirable outputs. You should expect to have to fine-tune the model for your use-case.**

Possible research areas and tasks include

- Applications in computer control or digital agents.
- Research on multi-modal models generally.

Excluded uses are described below.

### Out-of-Scope Use

The model was not trained to be factual or true representations of people or events, and therefore using the model to generate such content is out-of-scope for the abilities of this model.

## Limitations and Bias

### Limitations

- Faces and people in general may not be generated properly.

### Bias
While the capabilities of these models are impressive, they can also reinforce or exacerbate social biases.
