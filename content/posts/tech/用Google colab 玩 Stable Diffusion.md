---
title: "用Google colab 玩 Stable Diffusion"
date: 2023-05-08T18:34:25+08:00
draft: false
categories:
- CV
- 深度学习
tags:
- Stable Diffusion
- Google Colab
---


## 注册谷歌账号

实测用Firefox浏览器挂VPN能成功注册谷歌账号了，+86号码可用，如果不可用，可以用在线号码接收网站[# 便宜的虚拟电话号码](https://sms-activate.org/)辅助注册，YouTube上有教程。


## 在Google Colab+Drive中安装Stable Diffusion 


[Install the WebUI Colab to Google Drive](https://github.com/camenduru/stable-diffusion-webui-colab/tree/drive)

{{< youtube qN-pgCIScrw >}}

参照教程上面的依次运行即可

### 🦒 Install the WebUI Colab to Google Drive
文件体积较大，大概10G左右，提前确保google drive储存空间足够
Colab Page|Function
-|-
[![Open In Colab](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/github/camenduru/stable-diffusion-webui-colab/blob/drive/install.ipynb)|One Time Install & Update
[![Open In Colab](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/github/camenduru/stable-diffusion-webui-colab/blob/drive/run.ipynb)|Run
[![Open In Colab](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/github/camenduru/stable-diffusion-webui-colab/blob/drive/add.ipynb)|Add Model

### Tutorials

Stable Diffusion WebUI Colab With Google Drive: [https://www.youtube.com/watch?v=njW64feGMzI](https://www.youtube.com/watch?v=njW64feGMzI)

🚨 Important for 15G Free Google Drive Users:

If you have enough space, remove the # character before the line

[![image](https://user-images.githubusercontent.com/54370274/235278379-b7993166-e09d-48b9-ac0f-673a1262fcfd.png)](https://user-images.githubusercontent.com/54370274/235278379-b7993166-e09d-48b9-ac0f-673a1262fcfd.png)

If you want to use more models, you can download your model into Colab, which has an empty 50GB space.

Download models into `/content/models` WebUI automaticly will find your models under `/content/models` and `/content/drive/MyDrive/stable-diffusion-webui-colab/stable-diffusion-webui/models/Stable-diffusion`

[![Screenshot 2023-03-20 133516](https://user-images.githubusercontent.com/54370274/226315414-2dcc8308-c15f-4a96-8d75-507e46d5b1bc.png)](https://user-images.githubusercontent.com/54370274/226315414-2dcc8308-c15f-4a96-8d75-507e46d5b1bc.png)

!aria2c --console-log-level=error -c -x 16 -s 16 -k 1M https://huggingface.co/andite/pastel-mix/resolve/main/pastelmix-fp16.ckpt -d /content/models -o pastelmix-fp16.ckpt
!aria2c --console-log-level=error -c -x 16 -s 16 -k 1M https://huggingface.co/hakurei/waifu-diffusion-v1-4/resolve/main/vae/kl-f8-anime2.ckpt -d /content/models -o pastelmix-fp16.vae.pt

You can also free up more space by deleting the default model in your drive.

[![Screenshot 2023-02-24 140908](https://user-images.githubusercontent.com/54370274/221165025-706b6385-8cb2-4fe1-9334-2762013b9dce.png)](https://user-images.githubusercontent.com/54370274/221165025-706b6385-8cb2-4fe1-9334-2762013b9dce.png)

If you don't plan to use ControlNet models, you can also free up space by deleting them. (thanks to twitter@muadoyoki ❤)

[![Screenshot 2023-02-24 141259](https://user-images.githubusercontent.com/54370274/221165764-b4db7c3c-0cc9-4976-a922-395bb72a002d.png)](https://user-images.githubusercontent.com/54370274/221165764-b4db7c3c-0cc9-4976-a922-395bb72a002d.png)


## 从CiviTai下载模型

[Browse, share, and review custom AI art models——Civitai官网](https://civitai.com/)

### 常用模型LORA

{{< youtube XnfM0d2EIL0 >}}

原理：逐层加料

### 常用模型ControlNet

##  Diffusion扩散模型绘图原理

逆向降噪：从模糊到清晰

- 为何加噪点？降维，RGB像素点，保留重要特征点
- 如何降噪点？用采样器降维，采样步数：采样次数










