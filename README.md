# Are we there yet?

This repository contains code for reproducing the results reported in the following paper:

Orhan AE (2021) [How much "human-like" visual experience do current self-supervised learning algorithms need to achieve human-level object recognition?](https://arxiv.org/abs/2109.11523) arXiv:2109.11523.

## Usage

To train a self-supervised model with the temporal classification objective, e.g.:

```
python train_tc.py --model 'resnext101_32x8d' --n_out 16279 --class-fraction 1.0 --data-dirs DATA-DIRS
```

Here `class-fraction` is the fraction of the natural video data used for self-supervised learning (we use 100%, 10%, 1%, 0.1% of the data in the paper), `n_out` is the number of temporal episodes (this should be 16279 for 100% of the data, 1628 for 10% of the data, 163 for 1% of the data, and 17 for 0.1% of the data), and `data-dirs` is a list of directories containing the training data. Please see `train_tc.py` for additional arguments.

To evaluate a trained model on ImageNet, e.g.:
```
python finetune_imgnet.py --model 'resnext101_32x8d' --frac-retained 0.010147 --print-freq 25 --batch-size 512 --n_out 16279 --resume SAVED-MODEL-PATH
```
Here, `frac-retained` is the fraction of the labeled ImageNet training data used during fine-tuning. This should be set to 0.010147 for the `few-shot (~1%)` evaluation condition (fine-tuned with exactly 13000 labeled examples), 0.02 for the `few-shot (2%)`, and 1.0 for the linear probe evaluations. In addition, for the linear probe evaluations, set the flag `--freeze-trunk` (this will freeze the trunk of the pretrined model). Please see `finetune_imgnet.py` for additional arguments.

## Pretrained models

* The largest, highest performing self-supervised model with the temporal classification head attached (with 16279 outputs): [link](https://drive.google.com/file/d/1AL49wO-hc1m6N2KxKTSjHMTYUIXJkDfV/view?usp=sharing) (1.4 GB)
* The self-supervised model above with the ImageNet head instead (trained under the linear probe condition: 43.5% top-1): [link](https://drive.google.com/file/d/1faeTesYiBtEgLquI6hjFGzbzx0lslaMC/view?usp=sharing) (372.6 MB)

All other models for which results are reported in the paper are also privately saved in our local HPC cluster. However, I don't have enough disk space on my Google Drive at the moment to share them all publicly. Please let me know if you are interested in any of the other models. I would be happy to share them through other means.

## Data points

The raw data that went into Figure 1 are as follows:

Few-shot (~1%):
video_lengths_ft = [1.301, 1.301, 1.301, 13.01, 13.01, 13.01, 130.1, 130.1, 130.1, 1301, 1301, 1301]
acc_5_ft = [20.356, 20.356, 21.388, 31.688, 30.544, 29.692, 35.040, 35.486, 34.296, 42.958, 42.332, 42.426]
acc_1_ft = [8.012, 7.906, 8.720, 14.796, 14.298, 13.802, 17.160, 17.228, 16.578, 22.386, 21.980, 22.090]

Few-shot (2%):
video_lengths_ft_2 = [1.301, 1.301, 1.301, 13.01, 13.01, 13.01, 130.1, 130.1, 130.1, 1301, 1301, 1301]
acc_5_ft_2 = [34.238, 29.392, 30.076, 45.036, 43.330, 42.334, 47.408, 48.498, 47.636, 54.304, 54.530, 55.044]
acc_1_ft_2 = [16.108, 12.976, 13.392, 23.464, 22.008, 21.522, 25.692, 26.426, 25.566, 30.822, 30.988, 31.238]

Linear probe:
video_lengths_fz = [1.301, 1.301, 1.301, 13.01, 13.01, 13.01, 130.1, 130.1, 130.1, 1301]
acc_5_fz = [25.426, 21.740, 24.654, 45.316, 45.032, 42.262, 52.620, 57.828, 53.482, 66.740]
acc_1_fz = [11.892, 9.576, 11.168, 25.484, 25.316, 23.436, 31.028, 35.502, 31.536, 43.542]
