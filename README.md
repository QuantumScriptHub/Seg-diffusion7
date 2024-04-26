# Seg-diffusion
## Text-to-Image Diffusion Model for Open-Vocabulary Semantic Segmentation
![Static Badge](https://img.shields.io/badge/Apache-blue?style=flat&label=license&labelColor=black&color=blue)
![Static Badge](https://img.shields.io/badge/passing-green?style=flat&label=build&labelColor=black&color=green)
![Static Badge](https://img.shields.io/badge/passing-green?style=flat&label=circleci&labelColor=black&color=green)
![Static Badge](https://img.shields.io/badge/welcome-green?style=flat&label=PRs&labelColor=black&color=green)
![Static Badge](https://img.shields.io/badge/Python-green?style=flat&label=Language&labelColor=black&color=green)

## 🪧 [Comprehensive supplements](https://github.com/QuantumScriptHub/Seg-diffusion/blob/semantic_segmentation/result/seg_diffusion_ACM_MM_2024_SUPP.pdf)
For more details on the experimental setup and comprehensive visual contrast results, please refer to our extensive [supplementary materials](https://github.com/QuantumScriptHub/Seg-diffusion/blob/semantic_segmentation/result/seg_diffusion_ACM_MM_2024_SUPP.pdf).  

##  📢 Overview
<p align="justify">
  Open-Vocabulary semantic segmentation is a challenging computer vision task that labels each pixel within an image based on text descriptions. The recent advancements in this field are largely due to the increased model capacity.
However, these models often struggle with unfamiliar image content or unseen text descriptions, as their understanding of the visual language world is limited to the data they were trained on.
Text-to-image diffusion models, on the other hand, have shown an impressive capability in generating high-quality images along with a wide range of open-vocabulary language descriptions. This led us to explore whether the comprehensive priors captured in text-to-image diffusion models could improve the zero-shot generalization of open-vocabulary semantic segmentation. In this study, we present Seg-diffusion, a novel approach derived from Stable Diffusion and built upon its rich prior knowledge of both visual and linguistic domains.
This approach formulates open-vocabulary semantic segmentation as a denoising diffusion process from noisy masks to object masks.  Specifically, the object mask diffuses from ground-truth masks to a random distribution in latent space, and the model learns to reverse this noisy process to reconstruct object masks guided by text embeddings. Extensive experiments on several widely used open-vocabulary semantic segmentation benchmark datasets demonstrate that our proposed Seg-diffusion outperforms previous well-established methods. Moreover, Seg-diffusion can perform zero-shot transfer to unseen datasets, delivering top-tier results in open-vocabulary semantic segmentation.
</p>
  
<div style="display:flex; justify-content:space-between;">
    <img src="result/finaltrain.jpg" width="48%">
    <img src="result/finalinfer.jpg" width="48%">
</div>

The left image shows the training process, while the right one illustrates the inference process.
##  🚀 Modest surprise

<p align="center" style="margin: 0; padding: 0;">
    <img src="result/18.png" height="200" style="margin: 0; padding: 0;"/><img src="result/1.png" height="200" style="margin: 0; padding: 0;"/><img src="result/10.png" height="200" style="margin: 0; padding: 0;"/><img src="result/6.png" height="200" style="margin: 0; padding: 0;"/>
</p>

<p align="justify">
We present Seg-diffusion, a diffusion model and associated fine-tuning protocol for Open-Vocabulary semantic segmentation. Its core principle is to leverage the rich visual knowledge stored in modern generative image models. The order of the images from left to right in each row above is: Original, OVSeg, SAN, Ours, GT.
</p> 

More results can be viewed by clicking on [result](./result).

## ⬇ Datasets
**All datasets are available in public**.
* Download the DUTS-TR and DUTS-TE from [Here](http://saliencydetection.net/duts/#org3aad434)
* Download the DUT-OMRON from [Here](http://saliencydetection.net/dut-omron/#org96c3bab)
* Download the HKU-IS from [Here](https://i.cs.hku.hk/~yzyu/research/deep_saliency.html)
* Download the ECSSD from [Here](https://www.cse.cuhk.edu.hk/leojia/projects/hsaliency/dataset.html)
* Download the PASCAL-S from [Here](http://cbs.ic.gatech.edu/salobj/)
  
## 🛠️  Dependencies
```bash
* Python >= 3.8.x
* Pytorch >= 2.0.1
* diffusers >= 0.25.1
* pip install -r requirements.txt
```
## 📦 Checkpoint cache

By default, our [checkpoint](https://drive.google.com/file/d/1bfnhDv6KKCJrsIy6DBj_41CMIt9EFxlG/view?usp=sharing) and [Res2Net backbone](https://drive.google.com/file/d/1aNRStCeNGLVE8x1z-cxNa42f2u0-AvZI/view?usp=sharing) are stored in Google Drive.
You can click the link to download them and proceed directly with inference.

## ⚙ Configurations

#### Training

- --pretrained_model_name_or_path : [Pretrained model](https://huggingface.co/stabilityai/stable-diffusion-2/tree/main) path for stable-diffusion-2 from hugging face, you need to download it and place it in a local directory.
- --Res2Net_model_path : [Pretrained Res2Net backbone model](https://drive.google.com/file/d/1aNRStCeNGLVE8x1z-cxNa42f2u0-AvZI/view?usp=drive_link), you need to download it and place it in the corresponding local path.  
- --train_img_list : img_list.txt, including the absolute path of all train images.  
- --train_gt_list : gt_list.txt, including the absolute path of all ground truth masks.  
- --val_img : Path of the validation set of images.  
- --val_gt : Path of the validation set of ground truth masks.

#### Inference 

- --input_rgb_path : The local path of the image to be inferred.
- --output_dir : The output path of the image after inference.
- --stable_diffusion_repo_path : [Pretrained model](https://huggingface.co/stabilityai/stable-diffusion-2/tree/main) path for stable-diffusion-2 from hugging face, you need to download it and place it in a local directory.
- --pretrained_model_path : The path of the best checkpoint saved by the model you trained，you can also use the [checkpoint](https://drive.google.com/file/d/1bfnhDv6KKCJrsIy6DBj_41CMIt9EFxlG/view?usp=sharing) we trained, load them into a local path, and proceed with inference directly.
- --Res2Net_model_path :  [Pretrained Res2Net backbone model](https://drive.google.com/file/d/1aNRStCeNGLVE8x1z-cxNa42f2u0-AvZI/view?usp=drive_link), you need to download it and place it in the corresponding local path. 

The default settings are optimized for the best result. However, the behavior of the code can be customized:
- Trade-offs between the **accuracy** and **speed** (for both options, larger values result in better accuracy at the cost of slower inference.)
  - `--ensemble_size`: Number of inference passes in the ensemble. Default: 10.
  - `--denoise_steps`: Number of denoising steps of each inference pass. Default: 10.
- `--half_precision`: Run with half-precision (16-bit float) to reduce VRAM usage, might lead to suboptimal result.
- By default, the inference script resizes input images to the *processing resolution*, and then resizes the prediction back to the original resolution. This gives the best quality, as Stable Diffusion, from which Seg-diffusion is derived, performs best at 384x384 resolution.  
  - `--processing_res`: the processing resolution; set 0 to process the input resolution directly. Default: 384.
  - `--output_processing_res`: produce output at the processing resolution instead of upsampling it to the input resolution. Default: False.
- `--seed`: Random seed can be set to ensure additional reproducibility. Default: None (using current time as random seed).
- `--batch_size`: Batch size of repeated inference. Default: 0 (best value determined automatically).

## 💻 Testing on your images
### 📷 Prepare images
If you have images at hand, skip this step. Otherwise, download a few images from [Here](http://saliencydetection.net/duts/download/DUTS-TE.zip):
### 🎮 Training and  Inference
Run **train.sh** and **inference.sh** scripts for  training and  inference.
```bash
git clone https://github.com/QuantumScriptHub/Seg-diffusion.git
cd scripts
bash train.sh

cd scripts
bash inference.sh
```

## 🎫 License

This work is licensed under the Apache License, Version 2.0 (as defined in the [LICENSE](LICENSE.txt)).

By downloading and using the code and model you agree to the terms in the  [LICENSE](LICENSE.txt).

[![License](https://img.shields.io/badge/License-Apache--2.0-929292)](https://www.apache.org/licenses/LICENSE-2.0)

