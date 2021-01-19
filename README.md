# AttnGAN-revision for Colab

Pytorch implementation for reproducing AttnGAN results in the paper [AttnGAN: Fine-Grained Text to Image Generation
with Attentional Generative Adversarial Networks](http://openaccess.thecvf.com/content_cvpr_2018/papers/Xu_AttnGAN_Fine-Grained_Text_CVPR_2018_paper.pdf) by Tao Xu, Pengchuan Zhang, Qiuyuan Huang, Han Zhang, Zhe Gan, Xiaolei Huang, Xiaodong He. (This work was performed when Tao was an intern with Microsoft Research). 

<img src="framework.png" width="900px" height="350px"/>


### Dependencies
python 3.6

Pytorch

In addition, please add the project folder to PYTHONPATH and `pip install` the following packages:
- `python-dateutil`
- `easydict`
- `pandas`
- `torchfile`
- `nltk`
- `scikit-image`



**Data**

1. Download our preprocessed metadata for [birds](https://drive.google.com/open?id=1O_LtUP9sch09QH3s_EBAgLEctBQ5JBSJ) [coco](https://drive.google.com/open?id=1rSnbIGNDGZeHlsUlLdahj0RJ9oo6lgH9) and save them to `data/`
2. Download the [birds](http://www.vision.caltech.edu/visipedia/CUB-200-2011.html) image data. Extract them to `data/birds/`
3. Download [coco](http://cocodataset.org/#download) dataset and extract the images to `data/coco/`


**Training**
- Pre-train DAMSM models:
  - For bird dataset: `python pretrain_DAMSM.py --cfg cfg/DAMSM/bird.yml --gpu 0`
  - For coco dataset: `python pretrain_DAMSM.py --cfg cfg/DAMSM/coco.yml --gpu 1`
 
- Train AttnGAN models:
  - For bird dataset: `python main.py --cfg cfg/bird_attn2.yml --gpu 2`
  - For coco dataset: `python main.py --cfg cfg/coco_attn2.yml --gpu 3`

- `*.yml` files are example configuration files for training/evaluation our models.


**Pretrained Model**
- [DAMSM for bird](https://drive.google.com/open?id=1GNUKjVeyWYBJ8hEU-yrfYQpDOkxEyP3V). Download and save it to `DAMSMencoders/`
- [DAMSM for coco](https://drive.google.com/open?id=1zIrXCE9F6yfbEJIbNP5-YrEe2pZcPSGJ). Download and save it to `DAMSMencoders/`
- [AttnGAN for bird](https://drive.google.com/open?id=1lqNG75suOuR_8gjoEPYNp8VyT_ufPPig). Download and save it to `models/`
- [AttnGAN for coco](https://drive.google.com/open?id=1i9Xkg9nU74RAvkcqKE-rJYhjvzKAMnCi). Download and save it to `models/`

- [AttnDCGAN for bird](https://drive.google.com/open?id=19TG0JUoXurxsmZLaJ82Yo6O0UJ6aDBpg). Download and save it to `models/`
  - This is an variant of AttnGAN which applies the propsoed attention mechanisms to DCGAN framework. 

**Sampling**
- Run `python main.py --cfg cfg/eval_bird.yml --gpu 1` to generate examples from captions in files listed in "./data/birds/example_filenames.txt". Results are saved to `DAMSMencoders/`. 
- Change the `eval_*.yml` files to generate images from other pre-trained models. 
- Input your own sentence in "./data/birds/example_captions.txt" if you wannt to generate images from customized sentences. 

**Examples generated by AttnGAN [[Blog]](https://blogs.microsoft.com/ai/drawing-ai/)**

caption example
:-------------------------:
the medium sized bird has a dark grey color, a black downward curved beak, and long wings.
![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_0_g0.png) ![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_0_g1.png) ![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_0_g2.png)
![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_0_a0.png)
![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_0_a1.png)

the bird is dark grey brown with a thick curved bill and a flat shaped tail.
![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_1_g0.png) ![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_1_g1.png) ![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_1_g2.png)
![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_1_a0.png)
![](models/bird_AttnGAN2/Black_Footed_Albatross_0001_796111/0_s_1_a1.png)

No | caption example
:-------------------------:|:-------------------------:
2  |  the bird is dark grey brown with a thick curved bill and a flat shaped tail.
3  |  bird has brown body feathers, white breast feathers and black beak
4  |  this bird has a dark brown overall body color, with a small white patch around the base of the bill.
5  |  the bird has very long and large brown wings, as well as a black body and a long black beak.
6  |  it is a type of albatross with black wings, tail, back and beak, and has a white ring at the base of its beak.
7  |  this bird has brown plumage and a white ring at the base of its long, curved brown beak.
8  |  the entire body is dark brown, as is the bill, with a white band encircling where the bill meets the head.
9  |  this bird is gray in color, with a large curved beak.



**Validation**
- To generate images for all captions in the validation dataset, change B_VALIDATION to True in the eval_*.yml. and then run `python main.py --cfg cfg/eval_bird.yml --gpu 1`
- We compute inception score for models trained on birds using [StackGAN-inception-model](https://github.com/hanzhanggit/StackGAN-inception-model).
- We compute inception score for models trained on coco using [improved-gan/inception_score](https://github.com/openai/improved-gan/tree/master/inception_score).


**Reference**

- [StackGAN++: Realistic Image Synthesis with Stacked Generative Adversarial Networks](https://arxiv.org/abs/1710.10916) [[code]](https://github.com/hanzhanggit/StackGAN-v2)
- [Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks](https://arxiv.org/abs/1511.06434) [[code]](https://github.com/carpedm20/DCGAN-tensorflow)
