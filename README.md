# 嵌入式系統實驗 期末專題

| 學期 | 112學年度第2學期 | 授課教師 | 李棟村 |
| --- | --- | --- | --- |
| 學號 | B1121004 | 姓名 | 蔡毅  |
| 學號 | B1142050 | 姓名 |吳聿鎧 |
1. 專題題目：寶寶安全相機
2. 功能：主要就是可以偵測人臉的部位,利用樹梅派相機模組,抓取嬰幼兒臉部部分，眼睛、鼻子等部位,判斷出危險時自動發出警訊，例如：臉被遮住。
3. 實做平台：樹莓派
4. 技術使用：Python、OpenCV、mediapipe及GPIO
5. 系統架構流程圖：使用Python語法程式，主要函式庫為OpenCV、mediapipe及TensorFlow Lite

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/2b902006-f9f1-45f5-a272-f27a7e78e785" />

1. 軟硬體設計：樹莓派、相機、LED、蜂鳴器
2. 環境安裝、執行步驟：

## **步驟 1：確保系統已更新**

```bash
sudo apt update 
sudo apt upgrade -y
```

## **步驟 2：安裝必要的套件**

```bash
sudo apt install -y libcamera-apps v4l-utils
```

## **步驟 3：啟用 `camera` 介面**

**開啟 /boot/firmware/config.txt** 來修改設定：

```bash
sudo nano /boot/firmware/config.txt
```

在檔案底部新增：

camera_auto_detect=1  //允許自動偵測 Raspberry Pi Camera
dtoverlay=vc4-kms-v3d  //啟用新的影像驅動程式

## **步驟 4：重新啟動 Raspberry Pi**

```bash
sudo reboot
```

## **步驟 5：檢查攝像頭是否被識別**

```bash
#確認設備是否被系統偵測到
libcamera-hello --list-cameras
```

## **步驟 6：測試攝像頭**

```bash
#執行成功將打開一個預覽視窗，顯示攝像頭的即時畫面
libcamera-hello
```

```bash
#拍攝一張照片並儲存為test.jpg
libcamera-jpeg -o test.jpg
```

## **步驟 7：建立並啟用虛擬環境

```bash
sudo apt-get update
sudo apt-get install python3-venv
# 建立名為 opencv 的虛擬環境
python3 -m venv venv

# 進入虛擬環境
source venv/bin/activate
```

## **步驟 8：安裝必要套件

```bash
#在虛擬環境中安裝
pip install opencv-python
pip install imutils
pip install tflite-runtime

#檢查是否已成功安裝
pip list
```

## **步驟 9：建立影像訓練集

- 到[https://teachablemachine.withgoogle.com/官網](https://teachablemachine.withgoogle.com/train)

<img width="684" height="595" alt="image (1)" src="https://github.com/user-attachments/assets/002412c6-689d-4d2e-8a2d-011de87bbd8a" />

- 訓練與匯出模型
- 下載訓練好的模型
- 由MobaXterm傳輸，將模型放在之後要撰寫的程式資料夾中
- `model_u2.tflite`( 模型檔）以及 `labels2.txt`（標籤檔）

## **步驟 10：編譯程式並**執行測試

執行編譯的程式檔`face.py` 

```bash
vim face.py
python3 face.py
```

1. 實體照片、執行畫面：

(1)訓練模型畫面：

<img width="391" height="847" alt="Screenshot 2025-05-21 203258" src="https://github.com/user-attachments/assets/59928b3c-6856-4c82-89cf-4f024c33e444" />

臉沒被遮住

<img width="395" height="853" alt="Screenshot 2025-05-21 203245" src="https://github.com/user-attachments/assets/21ef2ccd-4cc4-4630-9545-6f92d89427f6" />

  臉被遮住口鼻

<img width="390" height="842" alt="Screenshot 2025-05-21 203159" src="https://github.com/user-attachments/assets/15066b3f-c479-4d5f-851c-512ef996cd9e" />

  臉全被遮住

(2)實作成果：

<img width="963" height="761" alt="image (2)" src="https://github.com/user-attachments/assets/067efefe-8958-4690-a772-e1771e3bfd98" />

  dangerous
  無偵測到人(小孩不在)
  
<img width="747" height="701" alt="image (6)" src="https://github.com/user-attachments/assets/9763a7c9-421f-48ce-9c1a-ac6c51489cc8" />

   LED亮，蜂鳴器響起

<img width="898" height="708" alt="image (3)" src="https://github.com/user-attachments/assets/f8641c6b-e5f3-48c1-8952-0dde4019a9a2" />

  safe

<img width="736" height="698" alt="image (7)" src="https://github.com/user-attachments/assets/37c1245f-2f04-4ceb-99c7-9f1e8e8819aa" />

  LED暗，蜂鳴器安靜

<img width="902" height="708" alt="image (4)" src="https://github.com/user-attachments/assets/6472c680-30bb-4a43-924c-a1c14d801701" />

  dangerous
  mouth/nose covered

<img width="735" height="695" alt="image (8)" src="https://github.com/user-attachments/assets/664b0738-5680-479f-b905-8ada2cdcb962" />

  LED亮，蜂鳴器響起

<img width="875" height="665" alt="image (5)" src="https://github.com/user-attachments/assets/288790cf-d201-49c2-ab3e-f0eb1b7adf5f" />

           dangerous
         fully covered

<img width="735" height="695" alt="image (9)" src="https://github.com/user-attachments/assets/0ec80744-7ed6-463f-9fd3-4c6f5a4f3978" />

   LED亮，蜂鳴器響起

9.影片：[**https://youtu.be/8ghSuyTrAWo**](https://youtu.be/8ghSuyTrAWo)

10.心得

蔡毅：

這次嵌入式系統的期末小專題，讓我對 AI 模組的訓練和應用有了更深入，從一開始的模型訓練流程、資料蒐集，到後續的參數調整和測試。此外，我們以樹莓派作為主要硬體平台，並整合蜂鳴器、LED 燈及Webcam來進行實作應用。雖然過程中遇到不少問題，例如硬體連接、模型準確率不穩定等，但一步步解決的過程讓我收穫非常多，整體實作過程不僅加深了我對 AI 模組運作原理的認識，也提升了我在樹莓派實作與周邊元件控制方面的實務能力。

聿鎧：

這次嵌入式系統課程的期末小專題，我們在Linux系統下操作樹莓派，學會了控制其GPIO腳位，並使用Python實作硬體控制，例如LED與感測器等。過程中也學習了基礎的模型訓練與應用，將AI結合嵌入式平台。這讓我對Linux環境下的軟硬體整合有更深入的理解，也提升了解決實作問題的能力。

11.參考資料來源 :

https://steam.oxxostudio.tw/category/python/ai/ai-facial-features.html

https://kamranicus.com/building-a-raspberry-pi-3-baby-monitor/?utm_source=chatgpt.com

https://ithelp.ithome.com.tw/articles/10215294
