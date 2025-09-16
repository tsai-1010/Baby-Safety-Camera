# 嵌入式系統實驗 期末專題

| 學期 | 112學年度第2學期 | 授課教師 | 李棟村 |
| --- | --- | --- | --- |
| 學號 | B1121004
B1142050 
 | 姓名 | 蔡毅
吳聿鎧
 |
1. 專題題目：寶寶安全相機
2. 功能：主要就是可以偵測人臉的部位,利用樹梅派相機模組,抓取嬰幼兒臉部部分，眼睛、鼻子等部位,判斷出危險時自動發出警訊，例如：臉被遮住。
3. 實做平台：樹莓派
4. 技術使用：Python、OpenCV、mediapipe及GPIO
5. 系統架構流程圖：使用Python語法程式，主要函式庫為OpenCV、mediapipe及TensorFlow Lite

![image.png](attachment:db8684e7-841d-4b29-851e-44b0ecb11b9a:image.png)

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

## **步驟 7：**建立並啟用虛擬環境

```bash
sudo apt-get update
sudo apt-get install python3-venv
# 建立名為 opencv 的虛擬環境
python3 -m venv venv

# 進入虛擬環境
source venv/bin/activate
```

## **步驟 8：**安裝必要套件

```bash
#在虛擬環境中安裝
pip install opencv-python
pip install imutils
pip install tflite-runtime

#檢查是否已成功安裝
pip list
```

## **步驟 9：**建立影像訓練集

- 到https://teachablemachine.withgoogle.com/官網

![image.png](attachment:d648deb5-2c17-4091-84fb-d0f7d44a571b:image.png)

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

![Screenshot 2025-05-21 203258.png](attachment:3f206146-42ce-481d-8193-0c15a5dcb508:Screenshot_2025-05-21_203258.png)

              臉沒被遮住

![Screenshot 2025-05-21 203245.png](attachment:fa6f9204-9068-4967-8622-0f5ca4be74cc:Screenshot_2025-05-21_203245.png)

           臉被遮住口鼻

![Screenshot 2025-05-21 203159.png](attachment:95b258eb-6cf9-421b-a5a8-d35694df559a:6076a041-43db-4990-ad40-1da397369c4c.png)

              臉全被遮住

(2)實作成果：

![image.png](attachment:4b831c5b-5eb0-4f4d-9a83-0a37873dd6d1:e43e0be2-763d-4dcd-8b12-cb0b1a95029b.png)

          dangerous

  無偵測到人(小孩不在)

![image.png](attachment:c375c835-c622-4b29-8cdd-d9564fbfb031:image.png)

   LED亮，蜂鳴器響起

![image.png](attachment:40c55a45-197f-4f20-a774-f8ca8e6025e5:image.png)

                  safe

![image.png](attachment:b6674654-caec-4af6-b1dd-37c5a9ef1857:image.png)

      LED暗，蜂鳴器安靜

![image.png](attachment:62b6635e-1f08-44c4-9412-fac48d579a89:image.png)

         dangerous

   mouth/nose covered

![image.png](attachment:b4fcbed0-ad6f-40f6-b88f-25955e544a99:image.png)

   LED亮，蜂鳴器響起

![image.png](attachment:7453b183-6717-4b81-8f50-8ffb0811de4c:image.png)

           dangerous

         fully covered

![image.png](attachment:b4fcbed0-ad6f-40f6-b88f-25955e544a99:image.png)

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
