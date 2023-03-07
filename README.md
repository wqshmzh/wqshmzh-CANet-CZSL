# Conditional Attribute Network CANet

This is the offical pytorch code for the paper:

**Learning Conditional Attributes for Compositional Zero-Shot Learning, CVPR 2023**

<p>
<img src="figures/diagram.png" width="400">
</p>

<p>
<img src="figures/architecture.png" width="800">
</p>

If you find this work interesting please cite

```
@inproceedings{wang2023learning,
  title={Learning Conditional Attributes for Compositional Zero-Shot Learning},
  author={Qingsheng Wang, Lingqiao Liu, Chenchen Jing, Hao Chen, Guoqiang Liang, Peng Wang, Chunhua Shen},
  booktitle={CVPR},
  year={2023}
}
```

All code was implemented using Python 3.10 and Pytorch 1.12 on Ubuntu.

## 1. Data Preparation

UT-Zappos50K: <http://vision.cs.utexas.edu/projects/finegrained/utzap50k/ut-zap50k-images.zip>

MIT-States: <http://wednesday.csail.mit.edu/joseph_result/state_and_transformation/release_dataset.zip>

C-GQA: <https://s3.mlcloud.uni-tuebingen.de/czsl/cgqa-updated.zip>

1. Download datasets UT-Zappos50K, MIT-States, and C-GQA and unzip them into a dataset folder, e.g., /home/XXX/datasets. Rename the dataset folder names as **ut-zap50k**, **mit-states**, and **cgqa**.
2. Download data splits for UT-Zappos50K and MIT-States at <https://www.senthilpurushwalkam.com/publication/compositional/compositional_split_natural.tar.gz>
3. Unzip the downloaded file **compositional_split_natural.tar.gz** and place the sub-folders **mit-states** and **ut-zap50k** into the corresponding dataset folder. Note that the cgqa dataset zip file contains its own data split.
4. Now, we have the following folder structure for the three datasets:

    ```python
    > /home/XXX
      > datasets
        > ut-zap50k # can also be mit-states or cgqa
        - metadata_compositional-split-natural.t7
        > compositional-split-natural
            - test_pairs.txt
            - train_pairs.txt
            - val_pairs.txt
        # ===Create this empty folder manually for UT-Zappos50K===# 
        > images
        # ======Only UT-Zappos50K has the following folders=======#
        > Boots
        > Sandals
        > Shoes
        > Slippers
    ```
5. Run **/utils/reorganize_utzap.py** to reorganize images in UT-Zappos50K, where set DATA_FOLDER='/home/XXX/datasets' in line 20.
6. (Optional) Delete sub-folders **Boots**, **Sandals**, **Shoes**, and **Slippers** in **ut-zap50k**.

## 2. Inference

We provide the trained parameters for all three datasets:

   Google Drive: <https://drive.google.com/drive/folders/1IGXPMRossFuVxIeWzvKRXrczXDNhHG1F?usp=sharing>

   Baidu Netdisk: <https://pan.baidu.com/s/1D3BaNKgTjy7dxI8fvcbbDA?pwd=2ity>

1. Download all trained parameters into the manually created folder **saved model**. Now we have the folder structure:
   
   ```
   > CANet-CZSL-master
     > ...
     > model
     > saved model
       - saved_cgqa.t7
       - saved_mit-states.t7
       - saved_ut-zap50k.t7
     > utils
     > ...
   ```

2. Open **test.py**, you have to specify some arguments before running this code: **args.dataset**, **args.data_root**, and **device** in lines 31-34.
3. Run this code. You will get exactly the same results reported in the paper.
   
   UT-Zappos50K:
   <p>
   <img src="figures/test_ut-zap50k.png" width="800">
   </p>

   MIT-States:
   <p>
   <img src="figures/test_mit-states.png" width="800">
   </p>

   C-GQA:
   <p>
   <img src="figures/test_cgqa.png" width="800">
   </p>


*Warning: If you were using an older version pytorch, you would get slightly different results because the pre-trained parameters of the image backbone is different from the lasted version pytorch.*

## 3. Training

You can train the model from scratch.

You can edit configurations for different runs in **flags.py** and **configs/dataset name/CANet.yml**. The file **flags.py** defines some of the shared hyper-parameters and the *.yml files define dataset-specific hyper-parameters.

Open **train.py** and set **args.dataset**, **args.data_root**, and **device** in lines 34-37 before your training.

You will notice that a folder named **logs** is created automatically after starting the training. This folder contains the saved checkpoint model, best model, tensorboard file, and all history results in a csv file.

## Acknowledgement

This code package is based on the CZSL library at <https://github.com/ExplainableML/czsl>. We show our deep appreciation for their contribution.
