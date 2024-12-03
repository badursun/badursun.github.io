---
layout: post
title: "Arduino DUE + CTE7 CPLD 7inch Screen + Ethernet Shield + Load Image From SD"
date: 2016-11-25 21:58
categories: ["Yazilim", "Embedded System"]
tags: ["arduino", "atmega", "arduino-mega", "cte7", "clpd-screen"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/cte7-cpld-66eea98694d3f.webp"
  alt: "Arduino DUE + CTE7 CPLD 7inch Screen + Ethernet Shield + Load Image From SD"
---

Hello,

I found out this solution by myself with an experience that I gained in a bit complicated ways. I didn't cross anybody else who had functioned this big screen by Arduino DUE before. Even in the forum only Graham (@ghlawrence2000) helped a bit. And at the end I succeed in transfering the display from an SD card to a screen by the DUE. Plus I achieved a speedy image change with a CPLD screen.

If you want see on action: [https://instagram.com/p/0cn_uUvdOO/](https://instagram.com/p/0cn_uUvdOO/)

You can find used libraries and Source Code and RAW image sample in the following link:

- [Source Code](https://www.dropbox.com/s/6689uavv9c7zkbd/CPLD_screen.rar?dl=0)
- [Used Libraries (UTFT, UTouch, sd, sd_raw etc..)](https://www.dropbox.com/s/fk6sysbadje6u3a/libraries.rar?dl=0)
- [RAW image sample for sd card](https://www.dropbox.com/s/t4nznrh6s2nlnky/SD_raw_image.rar?dl=0)

[Or see all files in folder (Dropbox)](https://www.dropbox.com/sh/skv0rdg1x3ofimo/AAAOEytnwqcMs8s6yWxkR-hEa?dl=0)

## Here are the materials that I used
- Arduino DUE
- Arduino ethernet shield  (Pic: [https://bit.ly/1BEKo3V](https://bit.ly/1BEKo3V) )
- 7inch CPLD + SDRAM TFT Touch Screen
- 2gb Micro SD Card

## Here is the diagram of Connection and Pin:

|LCD Screen        |   Arduino Due Pin No |
| DB0               |   37 |
| DB1               |   36 |
| DB2               |   35 |
| DB3               |   34 |
| DB4               |   33 |
| DB5               |   32 |
| DB6               |   31 |
| DB7               |   30 |
| DB8               |   22 |
| DB9               |   23 |
| DB10              |   24 |
| DB11              |   25 |
| DB12              |   26 |
| DB13              |   27 |
| DB14              |   28 |
| DB15              |   29 |
| RS                |   46 |
| WR                |   47 |
| RD                |   3.3V Pulled high |
| CS                |   44 |
| REST              |   45 |
| LED_A             |   +5V |
| GND               |   GND |
| 3.3V              |   +3.3V |

-----------------------------------------------------------------
TouchScreen      |   Arduino Due Pin No
-----------------------------------------------------------------
TCLK             |   40
TCS              |   41
TDIN             |   42
TDOUT            |   49
IRQ              |   38

### Here is the INIT Code.

```javascript
UTFT myGLCD(CPLD, 46, 47, 44, 45);
UTouch myTouch(40, 41, 42, 49, 38);
UTFT_SdRaw myFiles(&amp;myGLCD);
```

First article published on [arduino forum](https://forum.arduino.cc/index.php?topic=309817.0)