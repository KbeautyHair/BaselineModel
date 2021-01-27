## Backbone model: StarGAN v2
> **Paper**<br>
> StarGAN v2: Diverse Image Synthesis for Multiple Domains https://arxiv.org/abs/1912.01865<br>
> [Yunjey Choi](https://github.com/yunjey)\*, [Youngjung Uh](https://github.com/youngjung)\*, [Jaejun Yoo](http://jaejunyoo.blogspot.com/search/label/kr)\*, [Jung-Woo Ha](https://www.facebook.com/jungwoo.ha.921)<br>
> In CVPR 2020. (* indicates equal contribution)<br>

> **Official implementation (Pytorch) **<br>
> https://github.com/clovaai/stargan-v2<br>

> **TensorFlow implementation**<br>
> The TensorFlow implementation of StarGAN v2 by our team member junho can be found at [clovaai/stargan-v2-tensorflow](https://github.com/clovaai/stargan-v2-tensorflow).

## Software installation
Clone this repository:

```bash
git clone https://github.com/KbeautyHair/KbeautyBaseline.git
cd KbeautyBaseline/
```

Install the dependencies:
```bash
conda create -n KbeautyBaseline python=3.6.7
conda activate KbeautyBaseline
conda install -y pytorch=1.4.0 torchvision=0.5.0 cudatoolkit=10.0 -c pytorch
conda install x264=='1!152.20180717' ffmpeg=4.0.2 -c conda-forge
pip install opencv-python==4.1.2.30 ffmpeg-python==0.2.0 scikit-image==0.16.2
pip install pillow==7.0.0 scipy==1.2.1 tqdm==4.43.0 munch==2.5.0
```

## Datasets and pre-trained networks
We provide a script to download K-hairstyle dataset we used to train the baseline model and the corresponding pre-trained weights. The datasets and network checkpoints will be downloaded and stored in the `data` and `expr/checkpoints` directories, respectively.

To download the [K-hairstyle](link) dataset and the pre-trained weights, run the following commands:
```bash
bash download.sh k-hairstyle-dataset
bash download.sh pretrained-network-k-hairstyle
```

## Translating images
After downloading the pre-trained networks, you can synthesize output images reflecting diverse hairstyles of reference images. The following commands will save generated images to the `expr/results` directory. 
To generate images, run the following command:
```bash
python main.py --mode sample --num_domains 2 --resume_iter 100000 --w_hpf 1 \
               --checkpoint_dir expr/checkpoints/celeba_hq \
               --result_dir expr/results/celeba_hq \
               --src_dir assets/representative/celeba_hq/src \
               --ref_dir assets/representative/celeba_hq/ref
```

## Evaluation metrics
To evaluate the baseline model using [Fr&eacute;chet Inception Distance (FID)](https://arxiv.org/abs/1706.08500), run the following commands:
```bash
python main.py --mode eval --num_domains 2 --w_hpf 1 \
               --resume_iter 100000 \
               --train_img_dir data/celeba_hq/train \
               --val_img_dir data/celeba_hq/val \
               --checkpoint_dir expr/checkpoints/celeba_hq \
               --eval_dir expr/eval/celeba_hq

## Training networks
To train the baseline model from scratch, run the following commands. Generated images and network checkpoints will be stored in the `expr/samples` and `expr/checkpoints` directories, respectively. Training takes about three days on a single Tesla V100 GPU. Please see [here](https://github.com/clovaai/stargan-v2/blob/master/main.py#L86-L179) for training arguments and a description of them in the original StarGAN v2 implementation.

```bash
python main.py --mode train --num_domains 2 --w_hpf 1 \
               --lambda_reg 1 --lambda_sty 1 --lambda_ds 1 --lambda_cyc 1 \
               --train_img_dir data/celeba_hq/train \
               --val_img_dir data/celeba_hq/val
```

## License
The source code, pre-trained models are available under [Creative Commons BY-NC 4.0](https://github.com/clovaai/stargan-v2/blob/master/LICENSE) license by NAVER Corporation. You can **use, copy, tranform and build upon** the material for **non-commercial purposes** as long as you give **appropriate credit** by citing our paper, and indicate if changes were made. 

For business inquiries, please contact clova-jobs@navercorp.com.<br/>	
For technical and other inquires, please contact yunjey.choi@navercorp.com.


## Reference
If you want to get more details of the original model Stargan v2, [this repository](https://github.com/clovaai/stargan-v2) will be useful for your research
