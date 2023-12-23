---
title: 【AWS Terraform】 サブネットのパブリックIPv4アドレスを自動割り当て
tags:
  - AWS
  - Terraform
  - Network
private: true
updated_at: '2023-10-28T11:11:14+09:00'
id: 458efc0e5eabbf7cde60
organization_url_name: null
slide: false
ignorePublish: false
---

# Introduction

AWSでEC2に接続できない原因の典型として、対象のSubnetで`パブリックIPv4アドレスの自動割り当て`を有効化できていないことがあります。デフォルトでは有効化されません。

Terraformで環境構築時に上記事象が発生し、公式ドキュメントで少し探したため備忘としてまとめます。[^1]
[^1]:`ipv4`で検索して見つからなかったため、ページを間違えたかと思いました(map_public_ip_on_launchでした)。

なおAWSではデフォルトでVPCやSubnetが用意されており、こちらでは有効化されていますが、デフォルトの使用はアンチパターンのためお気をつけください。

**本記事が少しでも読者様の学びに繋がれば幸いです！**
**「いいね」をしていただけると今後の励みになるので、是非お願いします！**

## 環境

Ubuntu 22.04
Terraform v1.6.5

## 設定

公式ドキュメントのSubnetの`map_public_ip_on_launch`に`パブリックIPv4アドレスの自動割り当て`のフラグがあります。

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet

![Screenshot from 2023-12-23 23-55-00.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3292052/5b740f2a-deb0-27f6-d4d9-002d28c1366c.png)

> - map_public_ip_on_launch - (Optional) Specify true to indicate that instances launched into the subnet should be assigned a public IP address. Default is false.
>   <br>
> - (日本語訳): map_public_ip_on_launch - （オプション）サブネットに起動したインスタンスにパブリックIPアドレスを割り当てる必要がある場合はtrueを指定します。デフォルトはfalseです。

`true`にしたら完了です。

```hcl: subnet.tf
resource "aws_subnet" "public_1a" {
  vpc_id = var.vpc_id
  availability_zone = var.subnet_az[0]
  cidr_block              = var.public_subnet_cidr[0]
  # パブリックIPv4アドレスの自動割り当て
  map_public_ip_on_launch = true

  tags = {
    Name = "public-1a"
  }
}

```

### 最後に

最後まで閲覧頂きありがとうございました。

`パブリックIPv4アドレスの自動割り当て`は資格試験でも頻出のため、覚えておいて損はないと思います。

本記事がお役に立てば幸いです！

### 参考URL

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet
