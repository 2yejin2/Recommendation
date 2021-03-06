# User Diverse Preferences Modeling By Multimodal Attentive Metric Learning
Pytorch implementation of [User Diverse Preferences Modeling By Multimodal Attentive Metric Learning(Liu et al., 2019)](https://dl.acm.org/doi/abs/10.1145/3343031.3350953)

## Data preparation
#### 1) Amazon review dataset
Data should be prepared as follows
- train.csv (Rating data for train)
- test.csv (Rating data for test)
- image_feature.npy (Image features of items extracted from pretrained network)
- doc2vecFile (Parameters of model pretrained with text data of items)

Amazon Review(Office) Dataset can be downloaded here<br>
[Office dataset](https://drive.google.com/drive/folders/19pfDw8fpIfcI4B2oncygxo9OJKo9apG2?usp=sharing)

#### 2) Movielens dataset
Data should be prepared as follows
- movie_3953.ftr (Rating data)
- movies.csv (Information of movie)
- image_feature_vec.pickle (Image features of movie posters extracted from pretrained network)
- text_feature_vec.pickle (Text features of movie's title+plot extracted from pretrained network)

Movielens Dataset can be downloaded here<br>
[Movielens dataset](https://drive.google.com/drive/folders/15T7s2DDFt1HLlwRVw4ytViKE2rAAXgsj)


## Usage
```
# Train & Test Top 10 Recommendation with Multimodal information (Amazon)
CUDA_VISIBLE_DEVICES=0 python main.py --save_path <Your save path> --data_path <Your data path> --dataset amazon --top_k 10 --use_feature True
```
```
# Train & Test Top 10 Recommendation without Multimodal information (Movielens)
CUDA_VISIBLE_DEVICES=0 python main.py --save_path <Your save path> --data_path <Your data path> --dataset movielens --top_k 10 --use_feature False
```
For ```--use_feature False``` , Only embedding loss will be used and attention layer will have user&item latent vector as its input</br>

The following results will be saved in ```<Your save path>```
- train.log ( epoch, total loss, embedding loss, feature loss, covariance loss )
- test.log ( epoch, hit ratio, nDCG )
- model.pth (model saved every n epoch)


## Arguments
| Argument | Type | Description | Default | Paper |
|:---:|:---:|:---:|:---:|:---:|
|save_path|str|Path to save result|'./result'|-|
|data_path|str|Dataset|'amazon'|-|
|data_path|str|Path to dataset|'./Data/Office'|-|
|batch_size|int|Train batch size|1024|-|
|epoch|int|Train epoch|1000|maximum 1000|
|embed_dim|int|Dimension of latent vectors|64|64|
|dropout_rate|float|Dropout rate in feature fusion network|0.2|-|
|lr|float|Learning rate|0.001|0.0001~0.1|
|margin|float|Margin for embedding loss|1.6|1.6|
|feat_weight|float|Weight of feature loss|7|7|
|cov_weight|float|Weight of covariance loss|5|5|
|top_k|int|Top k Recommendation|10|10|
|num_neg|int|Number of negative samples for training|4|4|
|load_path|str|Path to saved model. Used to load checkpoint|None|-|
|use_feature|str2bool|Whether to use multimodal information|True|-|
|eval_freq|int|evaluate every n epoch|50|-|
|feature_type|str|Type of feature to use. ["all","img","txt"] |"all"|-|


## Result
Batch size : 1024</br>
Train Epoch : 1000</br>
| Dataset | Use Feature | Learning Rate | Margin | Feature loss weight | Covariance loss weight | HR@10 | nDCG@10 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Office | O | 0.001 | 1.6 | 7 | 5 | 14.1401 | 2.8405 |
| Office | X | 0.001 | 1.6 | - | - | 19.1046 | 4.7864 |




## Reference
[Official Code](https://github.com/liufancs/MAML#user-diverse-preferences-modeling-by-multimodal-attentive-metric-learning)
