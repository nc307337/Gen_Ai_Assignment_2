# Gen_Ai_Assignment_2

Comparative Study and Implementation of Generative AI Models

Assignment No. 2 (CO3 and CO4): AI-assisted comparative study of an Autoencoder (AE), a Variational Autoencoder (VAE), a Deep Convolutional GAN (DCGAN) and a pretrained Stable Diffusion model.

Group members

Name	PRN
Nilesh Chavan	202401110017
Bhusan Bhamre	202401110078
Project overview

The project compares two goals in image modelling:

Reconstruction (CO3): AE and VAE rebuild a given image from a compressed code.
Generation (CO4): DCGAN creates new images from random noise, and Stable Diffusion v1.5 creates images from text prompts.

AE, VAE and DCGAN are implemented in PyTorch and trained for 10 epochs on the Fashion-MNIST dataset (60,000 training and 10,000 test images, 28 x 28 grayscale). Stable Diffusion is pretrained and used for inference only, so the study compares what each model can do; it is not a same-domain image-quality benchmark.

Files
File	Description
Gen_Ai_Assignment_2.ipynb	Google Colab notebook with all code and outputs
Generative_AI_Comparative_Study_Report.docx	Short report (3 pages)
Generative_AI_Comparative_Study_Extended_Report.docx	Extended report (about 10 pages)
Generative_AI_Comparative_Study_Presentation.pptx	Presentation (5 slides)
Generative_AI_Comparative_Study_Presentation_10_Slides.pptx	Presentation (10 slides)
README.md	This file
How to run the notebook
Open the notebook in Google Colab.
Choose Runtime > Change runtime type and select a GPU (the results here used a Tesla T4). Stable Diffusion needs a GPU.
Run the cells from top to bottom. The first cell installs the extra libraries:
   pip install diffusers transformers accelerate safetensors
The notebook trains the AE, VAE and DCGAN, shows the figures, loads Stable Diffusion, generates four images, calculates the test MSE, and saves everything to a fashion_outputs folder (the last cell downloads it as a ZIP).

Main libraries: PyTorch 2.11.0, torchvision, matplotlib, numpy, diffusers, transformers.

Note: no random seed is set, so a re-run will give slightly different numbers.

Models and settings
Model	Architecture (as in the notebook)	Training
AE	784 -> 256 -> 64 -> 256 -> 784	Adam, lr 1e-3, MSE, 10 epochs
VAE	784 -> 400 -> mean and log-variance (20) -> 400 -> 784	Adam, lr 1e-3, BCE + KL, 10 epochs
DCGAN	Generator: Linear 100 -> 6272 and 2 transposed convs. Discriminator: 2 convs + linear	Adam, lr 2e-4, BCE, 10 epochs
Stable Diffusion	Pretrained runwayml/stable-diffusion-v1-5	Inference only: 4 prompts, 20 steps, guidance 7.0, 512 x 512

Batch size is 128 for all trained models.

Results summary
Result	Value
AE test MSE (10,000 images)	0.009628
VAE test MSE (10,000 images)	0.015939
AE loss, epoch 1 to 10	0.033584 to 0.009718
VAE loss per image, epoch 1 to 10	284.9157 to 242.3201
DCGAN discriminator / generator loss, epoch 10	0.5507 / 2.0316

Observations

The AE and VAE keep the overall shape of each item but lose fine detail. The VAE is visibly blurrier, which agrees with its higher test MSE.
The DCGAN produces recognisable shoes, tops and trousers with fairly sharp edges, but many samples are noisy or broken.
Stable Diffusion gives the most detailed and realistic images, with some prompt mismatches (for example, a red background instead of white for the red sneakers).
No numerical quality score (such as FID) was computed for DCGAN or Stable Diffusion, so those judgements are visual.
Limitations
Only 10 epochs of training and a single run.
Different data across the models (Stable Diffusion is not trained on Fashion-MNIST here).
AE and VAE differ in latent size (64 against 20) and in loss.
The Stable Diffusion safety checker was disabled for this experiment; its licence conditions still apply.
AI usage

ChatGPT and Claude were used to understand the models, write and debug the Colab code, and draft the report. Results, figures and settings were checked against the notebook's printed output.

References
G. E. Hinton and R. R. Salakhutdinov, "Reducing the dimensionality of data with neural networks," Science, 2006.
D. P. Kingma and M. Welling, "Auto-encoding variational Bayes," ICLR, 2014.
I. Goodfellow et al., "Generative adversarial nets," NeurIPS, 2014.
A. Radford, L. Metz and S. Chintala, "Unsupervised representation learning with deep convolutional generative adversarial networks," ICLR, 2016.
J. Ho, A. Jain and P. Abbeel, "Denoising diffusion probabilistic models," NeurIPS, 2020.
R. Rombach et al., "High-resolution image synthesis with latent diffusion models," CVPR, 2022.
H. Xiao, K. Rasul and R. Vollgraf, "Fashion-MNIST: a novel image dataset for benchmarking machine learning algorithms," 2017.
Hugging Face, "stable-diffusion-v1-5" model page: https://huggingface.co/stable-diffusion-v1-5/stable-diffusion-v1-5
