# SinGAN

[Project](https://tamarott.github.io/SinGAN.htm) | [Arxiv](https://arxiv.org/pdf/1905.01164.pdf) | [CVF](http://openaccess.thecvf.com/content_ICCV_2019/papers/Shaham_SinGAN_Learning_a_Generative_Model_From_a_Single_Natural_Image_ICCV_2019_paper.pdf) 

The original README is available [here](README_ORIGINAL.md).
### ICCV 2019 Best paper award (Marr prize) 受賞論文 "SinGAN: Learning a Generative Model from a Single Natural Image" のPyTorch実装


## 単一画像からのランダム画像生成
SinGANを用いることで、画像生成モデルを単一の画像で学習させることができ、その画像から新たな画像を生成することができる。例えば論文中の以下の画像を参照するとイメージがつく。

![](imgs/teaser.PNG)


## SinGANの応用例
SinGANは次のような一連の画像操作タスクにも使用することができる。
 ![](imgs/manipulation.PNG)
これは、学習済みモデルに画像を入力することによって行われます。
詳細は、[元論文](https://arxiv.org/pdf/1905.01164.pdf) の第4章を参照のこと。

### 引用
研究でこのコードを使う際には元論文を引用すること。

```
@inproceedings{rottshaham2019singan,
  title={SinGAN: Learning a Generative Model from a Single Natural Image},
  author={Rott Shaham, Tamar and Dekel, Tali and Michaeli, Tomer},
  booktitle={Computer Vision (ICCV), IEEE International Conference on},
  year={2019}
}
```

## コード

### 依存パッケージ

```
python -m pip install -r requirements.txt
```

本コードは、以下の環境で動作を確認している。
* Python 3.6
* PyTorch 1.6.0
* torchvision 0.7.0
* CUDA Toolkit 10.1

###  学習
SinGANモデルを任意の画像で学習させるには、Input/Images配下に画像を配置した上で下記コードを実行する。

```
python main_train.py --input_name <input_file_name>
```

CPU上で本コードを実行する際には、`--not_cuda` オプションを付与する。

###  ランダム画像生成
ランダム画像生成を行うためには、まず上述のように任意の画像によってSinGANモデルを学習させる。その後、以下のコードを実行する。

```
python random_samples.py --input_name <training_image_file_name> --mode random_samples --gen_start_scale <generation start scale number>
```

注記：フルモデルを使用したい場合には、開始スケールが '0' になるよう指定する。同様に、第2スケールから生成を行いたい場合は '1' になるよう指定する。

###  任意サイズのランダム画像生成
使用方法は通常のランダム画像生成と同様。

```
python random_samples.py --input_name <training_image_file_name> --mode random_samples_arbitrary_sizes --scale_h <horizontal scaling factor> --scale_v <vertical scaling factor>
```

###  調和

入力画像中にペーストされた対象物を画像に "調和" ([元論文](https://arxiv.org/pdf/1905.01164.pdf) Fig. 13 参照) させたい場合には、まず上述の学習を実施し、その後単に対象物を学習画像中にペーストした画像とそのマスク画像を "Input/Harmonization" 以下に配置（保存されている画像を見てみるとイメージが掴める）し、以下のコードを実行する。

```
python harmonization.py --input_name <training_image_file_name> --ref_name <naively_pasted_reference_image_file_name> --harmonization_start_scale <scale to inject>

```

注記：異なるスケールを用いて "調和" させると、それぞれで異なった調和効果が得られる。指定できる中で最も粗いスケールは '1' である。

###  画像編集

画像を "編集" ([元論文](https://arxiv.org/pdf/1905.01164.pdf) Fig. 12 参照) するには、まず編集されていない画像を用いて上述の学習を実施し、その後単純な編集を施した画像とそのマスク画像を "Input/Editing" 以下に配置（保存されている画像を見てみるとイメージが掴める）し、以下のコードを実行する。

```
python editing.py --input_name <training_image_file_name> --ref_name <edited_image_file_name> --editing_start_scale <scale to inject>

```
実行結果として、マスク出力、アンマスク出力の双方が保存される。

注記：異なるスケールを用いて "編集" すると、それぞれで異なった編集効果が得られる。指定できる中で最も粗いスケールは '1' である。

