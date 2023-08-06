# 使用python + opencv + pytesseract 來分析captcha

## 必要套件

 * PIL => pip install pillow
 * numpy => pip install numpy
 * cv2 => 	pip install opencv-python
 * pytesseract => pip install pytesseract

### tesseract for Windows 

使用 tesseract 工具必須要先從官方網站下載[安裝檔](https://digi.bib.uni-mannheim.de/tesseract/tesseract-ocr-w64-setup-5.3.1.20230401.exe)，或者從[網站下載](https://digi.bib.uni-mannheim.de/tesseract/)其他版本。

點擊安裝下載後的安裝檔，預設安裝路徑為**C:\Program Files\Tesseract-OCR**，必須要在系統環境變數增加此路徑，否則pytesseract會找不到執行檔。

## 開始寫程式處理

以下程式會從檔案中讀取圖片，經由處理後轉為另外的檔案，再丟給*pytesseract*去判讀。


```python
def ocr(filename):
	img = cv2.imread(filename, cv2.IMREAD_GRAYSCALE)

	img = cv2.GaussianBlur(img, (3, 3), 0)
	img = cv2.threshold(img, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]

	kernel = numpy.ones((1,1), numpy.uint8)
	img = cv2.dilate(img, kernel, iterations=1)

	cv2.imwrite('processed.jpg', img)


	# psm: 
	#  6    Assume a single uniform block of text.
	#  8    Treat the image as a single word.
	text = pytesseract.image_to_string(
		img, lang='eng', config="--psm 6 --oem 3 -c tessedit_char_whitelist=0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ")
```

### 參數

#### psm

![](./images/20230806130902473.png)  

psm 是最重要的參數，用來決定圖片中的文字形式，以上方我們要處理的圖片為例，使用不同的參數，可能會判讀失敗，或是判讀不正確，例如：  

* psm:6 => 判讀為**YEP8**
* psm:8 => 判讀為**YEPS**

不同的psm參數，導致正確資料從**8變成s**。

#### oem

#### tessedit_char_whitelist

告訴tesseract允許的字元範圍


#### 參考資料

* [captcha_solver.py	](https://gist.github.com/MrAch26/5e2aa7e73b508f8ba9133d468efa4348)
* [Simple Captcha Solver with Python](https://stackoverflow.com/questions/74642350/simple-captcha-solver-with-python/74654843#74654843)
* [Pytesseract : "TesseractNotFound Error: tesseract is not installed or it's not in your path", how do I fix this?](https://stackoverflow.com/a/53672281)
* [Unable to OCR alphanumerical image with Tesseract](https://stackoverflow.com/a/68942691/6408366)
* [Pytesseract OCR multiple config options](https://stackoverflow.com/questions/44619077/pytesseract-ocr-multiple-config-options)