# 1. 前言

随着移动互联网的发展，我们经常在火车票、汽车票、快餐店、电影院、团购网站以及移动支付等各个场景下见到二维码的应用，可见二维码以经渗透到人们生活的各个方面。条码、二维码以及RFID被人们应用的更普遍一些，二维码相对一维码，具有数据存储量大，保密性好等特点，能够更好地与智能手机等移动终端相结合，形成了更好地互动性和用户体验。而与RFID相比较，二维码不仅成本优势凸显，他的用户体验和互动性也具有更好地应用前景。

作为物联网浪潮产业下的一个环节，二维码的技术应用解决方案层出不穷，二维码已成移动互联网的入口。

# 2. 基础知识

## 二维码

[二维码](https://baike.baidu.com/item/%E4%BA%8C%E7%BB%B4%E7%A0%81/2385673?fr=aladdin)又称QR Code，QR全称Quick Response，是一个近几年来移动设备上超流行的一种编码方式，它比传统的Bar Code条形码能存更多的信息，也能表示更多的数据类型。

二维条码/二维码（2-dimensional bar code）是用某种特定的几何图形按一定规律在平面（二维方向上）分布的黑白相间的图形记录数据符号信息的；在代码编制上巧妙地利用构成计算机内部逻辑基础的“0”、“1”比特流的概念，使用若干个与二进制相对应的几何形体来表示文字数值信息，通过图象输入设备或光电扫描设备自动识读以实现信息自动处理：它具有条码技术的一些共性：每种码制有其特定的字符集；每个字符占有一定的宽度；具有一定的校验功能等。同时还具有对不同行的信息自动识别功能、及处理图形旋转变化点。

## QRCode

[QR Code](https://baike.baidu.com/item/QRCode/10336647?fr=aladdin&fromid=10462339&fromtitle=QR+Code)码，是由Denso公司于1994年9月研制的一种矩阵二维码符号，它具有一维条码及其它二维条码所具有的信息容量大、可靠性高、可表示汉字及图象多种文字信息、保密防伪性强等优点。


# 3. 利用Java生成解析二维码技术实现

- 在github上下载[zxing](https://github.com/zxing/zxing)，最新版本为3.3.1

下载后的文件目录，我们主要需要`core`和`javase`目录下的核心代码

![image](https://raw.githubusercontent.com/Echoingursb/gallery/master/%E4%BD%BF%E7%94%A8zxing%E7%94%9F%E6%88%90%E8%A7%A3%E6%9E%90%E4%BA%8C%E7%BB%B4%E7%A0%81/eejqmkx0ozg5wx4580o46r.png)

- 生成jar包

将`core`和`javase`文件下的`com`包在eclipse里导出jar包

![image](https://raw.githubusercontent.com/Echoingursb/gallery/master/%E4%BD%BF%E7%94%A8zxing%E7%94%9F%E6%88%90%E8%A7%A3%E6%9E%90%E4%BA%8C%E7%BB%B4%E7%A0%81/z3mrvzkm15uagtk4dza5o8.png)

- 创建项目

项目目录

![image](https://raw.githubusercontent.com/Echoingursb/gallery/master/%E4%BD%BF%E7%94%A8zxing%E7%94%9F%E6%88%90%E8%A7%A3%E6%9E%90%E4%BA%8C%E7%BB%B4%E7%A0%81/j5qltzteyyv9eewof1rbj2.png)

-生成二维码

**CreateQRCode核心代码**
```bash
/**
 * @author echo
 */
package com.echo.zxing;

import java.io.File;
import java.nio.file.Path;
import java.util.HashMap;

import com.google.zxing.BarcodeFormat;
import com.google.zxing.EncodeHintType;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;

public class CreateQRCode {

    public static void main(String[] args) {
        int width = 300;
        int height = 300;
        String format = "png";
        String content = "https://echoingursb.github.io/";

        // 定义二维码的参数
        HashMap hints = new HashMap();
        hints.put(EncodeHintType.CHARACTER_SET, "utf-8");
        hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.M);
        hints.put(EncodeHintType.MARGIN, 2);

        // 生成二维码
        try {
            BitMatrix bitMatrix = new MultiFormatWriter().encode(content, BarcodeFormat.QR_CODE, width, height, hints);
            Path file = new File("E:/ada/code/img.png").toPath();
            MatrixToImageWriter.writeToPath(bitMatrix, format, file);
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

    }

}

```
生成图片结果

![image](https://raw.githubusercontent.com/Echoingursb/gallery/master/%E4%BD%BF%E7%94%A8zxing%E7%94%9F%E6%88%90%E8%A7%A3%E6%9E%90%E4%BA%8C%E7%BB%B4%E7%A0%81/24sh3k9yt9tbr3vfgsyav9.png)

- 解析二维码

**ReadQRCode核心代码**


```bash
/**
 * @author echo
 */
package com.echo.zxing;

import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.HashMap;

import javax.imageio.ImageIO;

import com.google.zxing.BinaryBitmap;
import com.google.zxing.EncodeHintType;
import com.google.zxing.MultiFormatReader;
import com.google.zxing.NotFoundException;
import com.google.zxing.Result;
import com.google.zxing.client.j2se.BufferedImageLuminanceSource;
import com.google.zxing.common.HybridBinarizer;

public class ReadQRCode {
    public static void main(String[] args) {
        MultiFormatReader multiFormatReader = new MultiFormatReader();
        File file = new File("E:/ada/code/img.png");
        BufferedImage image = null;
        try {
            image = ImageIO.read(file);
        } catch (IOException e1) {
            // TODO Auto-generated catch block
            e1.printStackTrace();
        }
        BinaryBitmap binaryBitmap = new BinaryBitmap(new HybridBinarizer(new BufferedImageLuminanceSource(image)));

        // 定义二维码的参数
        HashMap hints = new HashMap();
        hints.put(EncodeHintType.CHARACTER_SET, "utf-8");
        try {
            Result result = multiFormatReader.decode(binaryBitmap, hints);
            System.out.println("解析结果：" + result.toString());
            System.out.println("二维码类型：" + result.getBarcodeFormat());
            System.out.println("二维码文本内容：" + result.getText());
        } catch (NotFoundException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

    }

}

```
解析图片结果

![image](https://raw.githubusercontent.com/Echoingursb/gallery/master/%E4%BD%BF%E7%94%A8zxing%E7%94%9F%E6%88%90%E8%A7%A3%E6%9E%90%E4%BA%8C%E7%BB%B4%E7%A0%81/f16ya5p9g0wv7esomaatid.png)

# 传送门

- [github项目地址](http://note.youdao.com/)
- [QR二维码日文官网](http://www.qrcode.com/zh/index.html)