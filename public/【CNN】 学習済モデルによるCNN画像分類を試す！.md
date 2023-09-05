---
title: 【CNN】 学習済モデルによるCNN画像分類を試す！
tags:
  - Python
  - 機械学習
  - pillow
  - CNN
  - PyTorch
private: false
updated_at: '2023-05-21T12:22:11+09:00'
id: 60ee9d7f12ca0a593365
organization_url_name: null
slide: false
---
# Introduction

モデルの汎化性能や精度の検証で、テストデータのみ学習時と変更して評価する手法があります。

再度`CNN`学習を実行するのは学習時間がかかるため、作成したモデルを用いた推論を備忘録を兼ねて紹介します。

`Resnet`等の学習済モデル、自身で`fine-tuning`等したモデルどちらでも可能です。

使用例として、以下のような場合に役立ちます。
- 新しいテスト用のデータセットで汎化性能を確認したい。
- `Kaggle`等のオープンデータセットに誤って混ざった、クラス外の画像を除去したい。
- 大量の生データを分類したい。

撮り過ぎたスマホの画像整理にも応用可能なため、日常にも使用シーンはありそうです。

本記事でもコードを紹介しますが、`GitHub`にも掲載しております。

https://github.com/kagami-tsukimura/cnn-image-classification

__本記事が少しでも読者様の学びに繋がれば幸いです！__
__「いいね」をしていただけると今後の励みになるので、是非お願いします！__

## 環境
Ubuntu22.04
Python3.11.1

## 学習済みモデル

本記事及び`GitHub`では`ResNet-18`を使用して紹介します。
ご自身でお試しする際は、分類内容に合わせて他のモデルに置き換えて利用可能です。

`torchvision`を用いているため、対応モデルは以下を参照ください。

https://pytorch.org/vision/stable/models.html

## 実装

先に全体を掲載します。
`./input`に分類したい画像を置いて以下のコードを実行すれば、画像分類されます。

```python: cnn_eval.py
import json
import subprocess
from glob import glob
from pathlib import Path
from tqdm import tqdm

import torch
import torchvision
from torch.utils.data import DataLoader, Dataset
from torchvision import transforms
from torchvision.datasets.utils import download_url
from PIL import Image


class CustomDataset(Dataset):
    """Custom dataset class."""

    def __init__(self, img_paths, transform):
        """Initialize the dataset
        Args:
            img_paths: image paths
            transform: data transform
        """
        self.img_paths = img_paths
        self.transform = transform

    def __getitem__(self, index):
        """Returns one data pair (image and caption).
        Args:
            index: index
        Returns:
            img: image
            img_path: image path
        """

        img_path = self.img_paths[index]
        img = Image.open(img_path).convert("RGB")
        img = self.transform(img)
        return img, img_path

    def __len__(self):
        """Returns the total number of image files.
        Returns:
            len: length
        """

        return len(self.img_paths)


def prepare_data():
    """Prepare data.
    Returns:
        test_transform: test data transform
    """

    test_transform = transforms.Compose(
        [
            transforms.Resize(256),
            transforms.CenterCrop(224),
            transforms.ToTensor(),
            # The values calculated from the ImageNet dataset.
            transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
        ]
    )
    return test_transform


def get_classes(CLASS_JSON):
    """Get class names.
    Args:
        CLASS_JSON: class json file
    Returns:
        class_names: class names
    """

    json_path = f"data/{CLASS_JSON}"
    if not Path(json_path).exists():
        # If there is no file, download it.
        download_url("https://git.io/JebAs", "data", CLASS_JSON)

    # Read the class list.
    with open(json_path) as f:
        data = json.load(f)
        class_names = [x["ja"] for x in data]

    return class_names


def mkdir(OUTPUT, dir):
    """Create directory.
    Args:
        dir: directory path
    """

    cmd = f"mkdir -p {OUTPUT}/{dir}"
    subprocess.call(cmd.split())


def mv_file(img, OUTPUT, dir):
    """Move file.
    Args:
        img: image path
        dir: directory path
    """

    cmd = f"mv {img} {OUTPUT}/{dir}"
    subprocess.call(cmd.split())


def main():
    """Main function."""

    CLASS_JSON = "imagenet_class_index.json"
    INPUT = "input"
    OUTPUT = "output"
    IMG = ".[jp][pn]g"

    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    model = torchvision.models.resnet18(pretrained=True).to(device)

    test_transform = prepare_data()
    imgs = glob(f"./{INPUT}/*{IMG}")
    dataset = CustomDataset(imgs, test_transform)
    dataloader = DataLoader(dataset, batch_size=8, shuffle=False)
    class_names = get_classes(CLASS_JSON)

    print("Start evaluation...")
    for images, img_paths in tqdm(dataloader):
        images = images.to(device)
        model.eval()
        with torch.no_grad():
            output = model(images)

        _, batch_indices = output.sort(dim=1, descending=True)

        for indices, img_path in zip(batch_indices, img_paths):
            dir = class_names[indices[0]]
            mkdir(OUTPUT, dir)
            mv_file(img_path, OUTPUT, dir)
    print("Finish evaluation!")


if __name__ == "__main__":
    main()

```

それでは、順にコードを追っていきます。

1. スクリプトに必要なライブラリをimportします。

    ```python: cnn_eval.py
    import json
    import subprocess
    from glob import glob
    from pathlib import Path
    from tqdm import tqdm
    
    import torch
    import torchvision
    from torch.utils.data import DataLoader, Dataset
    from torchvision import transforms
    from torchvision.datasets.utils import download_url
    from PIL import Image
    ```

1. データセットクラスを定義します。
このクラスでデータセットの読み込み(`Image.open`)と前処理(`transform`)を行います。

    ```python: cnn_eval.py
    class CustomDataset(Dataset):
        """Custom dataset class."""
    
        def __init__(self, img_paths, transform):
            """Initialize the dataset
            Args:
                img_paths: image paths
                transform: data transform
            """
            self.img_paths = img_paths
            self.transform = transform
    
        def __getitem__(self, index):
            """Returns one data pair (image and caption).
            Args:
                index: index
            Returns:
                img: image
                img_path: image path
            """
    
            img_path = self.img_paths[index]
            img = Image.open(img_path).convert("RGB")
            img = self.transform(img)
            return img, img_path
    
        def __len__(self):
            """Returns the total number of image files.
            Returns:
                len: length
            """
    
            return len(self.img_paths)
    ```

1. テストデータの前処理をして準備します。
    - `Resnet-18`は`224*224`の入力を受け付けるためサイズを調整します。
    - `Resnet-18`は`ImageNet`データセットで学習しているため、正規化も事前に計算された平均値と標準偏差を使用しています。

    ```python: cnn_eval.py
    def prepare_data():
        """Prepare data.
        Returns:
            test_transform: test data transform
        """
    
        test_transform = transforms.Compose(
            [
                transforms.Resize(256),
                transforms.CenterCrop(224),
                transforms.ToTensor(),
                # The values calculated from the ImageNet dataset.
                transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
            ]
        )
        return test_transform
    ```

1. クラス名を取得します。
`ImageNet`データセットのクラス名をWebサイトから一覧で取得します。

    ```python: cnn_eval.py
    def get_classes(CLASS_JSON):
        """Get class names.
        Args:
            CLASS_JSON: class json file
        Returns:
            class_names: class names
        """
    
        json_path = f"data/{CLASS_JSON}"
        if not Path(json_path).exists():
            # If there is no file, download it.
            download_url("https://git.io/JebAs", "data", CLASS_JSON)
    
        # Read the class list.
        with open(json_path) as f:
            data = json.load(f)
            class_names = [x["ja"] for x in data]
    
        return class_names
    ```

1. 画像分類で推論したクラス名のディレクトリがなければ作成します。
該当画像のディレクトリを移動します。

    ```python: cnn_eval.py
    def mkdir(OUTPUT, dir):
        """Create directory.
        Args:
            dir: directory path
        """
    
        cmd = f"mkdir -p {OUTPUT}/{dir}"
        subprocess.call(cmd.split())
    
    
    def mv_file(img, OUTPUT, dir):
        """Move file.
        Args:
            img: image path
            dir: directory path
        """
    
        cmd = f"mv {img} {OUTPUT}/{dir}"
        subprocess.call(cmd.split())
    ```

1. メイン処理です。
    - `model = torchvision.models.resnet18(pretrained=True).to(device)`で`ResNet-18`を指定しています。
        - 他の学習済みモデルをダウンロードする場合は上記を書き換えてください。
        - 自作のモデルであれば、以下のように書き換えると読み込み可能です。
            - `model.load_state_dict(torch.load(<自作モデルのパス>))`

```python: cnn_eval.py
def main():
    """Main function."""

    CLASS_JSON = "imagenet_classes.json"
    INPUT = "input"
    OUTPUT = "output"
    IMG = ".[jp][pn]g"

    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    model = torchvision.models.resnet18(pretrained=True).to(device)

    test_transform = prepare_data()
    imgs = glob(f"./{INPUT}/*{IMG}")
    dataset = CustomDataset(imgs, test_transform)
    dataloader = DataLoader(dataset, batch_size=8, shuffle=False)
    class_names = get_classes(CLASS_JSON)

    print(f"Input images: {len(imgs)}")
    print("Start evaluation...")
    for images, img_paths in tqdm(dataloader):
        images = images.to(device)
        model.eval()
        with torch.no_grad():
            output = model(images)

        _, batch_indices = output.sort(dim=1, descending=True)

        for indices, img_path in zip(batch_indices, img_paths):
            dir = class_names[indices[0]]
            mkdir(OUTPUT, dir)
            mv_file(img_path, OUTPUT, dir)
    print("Finish evaluation!")


if __name__ == "__main__":
    main()
```

## お試し

スクリプトを試していきます。
今回のスクリプトでは`ImageNet`データセットのクラスを分類できます。
お試し用に`icrawler`でブラウザから画像をクロールしました。

https://qiita.com/kagami_t/items/22e8c5eb95ee17a2353c

取得した画像を`./input`に格納します。

![Screenshot from 2023-05-21 11-34-39.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/b7da645a-0d06-5653-6b1d-a92f03664fd5.png)

個人的に好きなクラスの画像を集めただけで、選択に深い意味はありません。
- `fountain(噴水)`
- `hummingbird(ハチドリ)`
- `king_penguin(キングペンギン)`
- `Persian_cat(ペルシャ猫)`
- `shoe_shop(靴屋)`

スクリプトを実行します。

```bash:
python3 cnn_eval.py
```

実行後、`./output`にクラス名のディレクトリが作成されます。

![Screenshot from 2023-05-21 11-36-48.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/a7c1442a-898f-75e5-c308-2288f474cdeb.png)

ディレクトリ内を見てみると、綺麗に分類されています。

![Screenshot from 2023-05-21 11-39-30.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/2f30f83b-fe89-471f-4964-aefbe0eaddde.png)

![Screenshot from 2023-05-21 11-40-07.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/f8143408-8fb7-4baf-d2ec-a167e355ae13.png)

![Screenshot from 2023-05-21 11-41-07.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/37322145-9c5e-c7ff-8348-7b3d9ebe74a2.png)

![Screenshot from 2023-05-21 11-41-29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/9e423bb9-10e9-8ab7-937b-31f3cdeba7f2.png)

今回のテストデータに対して、`ResNet-18`の精度は問題ないことがわかります。

実務のデータはここまで綺麗ではないことが多いと思うので、この分類でモデルの精度やデータの整理を確認することになります。


## 最後に

`CNN`の画像分類は機械学習分野の中では理解しやすい分野だと思っています。
紹介したとおりコードも単純です。

オンボードのノートPC等でも`CPU`モードで動作するコードにしてあるため、是非お試ししてみてください。

最後まで閲覧頂きありがとうございました。
備忘録の側面もありますが、本記事がお役に立てば幸いです！


### 参考URL

- `GitHub`サンプルスクリプト

https://github.com/kagami-tsukimura/cnn-image-classification

- `ImageNet`クラス名一覧
    
https://git.io/JebAs
