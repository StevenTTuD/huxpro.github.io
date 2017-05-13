---
layout: post
title: CSS排版技巧 - 文繞圖
published: true
date: 2015-11-12 16:16
tags: []
categories: []
comments: true

---
## 文繞圖

先看效果，這次要做的是文繞圖技巧，可以選擇靠左或靠右。

![](https://lh3.googleusercontent.com/oP3fnp8f8uhuc0mxk_0mU9xYtmakaKKgF_Wc6_GeBrsKZl4CbQj8gR81SreC_lD2Wge2f2CDdmq4sx8ywUJSPdsN1P7GC1W0196lhH0VMLt1fUQ8wpnvUlxcqqJgvWwv7k-wnp5Zvptd234xOCVoeVD1EgluELO_c2pcejlRwMZqUGcufr_o1o1DpsmsqrPm0iQ-T7bgNskMImC4ZaEnfiE2J43jpgR6fknJ5rvYSpAP1rUOsS-n7UTxVyVZeFwJl7SrjCxmVQAQQ-dsDSWxvFKIfNcktfpEYFrm5oUf8AFWue-66DFGoZ4gXPV2LpBpyiqh4GA5q535SGNy01jrZKCKzEv-jIwwscd1n7dEfWFBlVZsfvxQ-SEm1E1fdaTZafF15bSmRxQlae96ibtUUYy4zSaEa5g2ycBFELJ61gmABHehSLsGYQwtZjeM-a4SESSwQSAtxiGIN2p6rz46wLPRK6y5JKkflhazE9gSK88dUVD5XnBuP4JiBcBnmg-RQJ8o_cT-NE0LgspEZ_MkB7Q6X1L2ci--utG7u6TVVxo=w1805-h761-no)

###html結構

圖片(img)必須在段落(p)的上面，當圖片加上`float:left`屬性時，圖片的空間會消失，變成浮在畫面上，利用這個技巧，我們來做到文繞圖的效果。

```html
<div class="image-container">
      <img class="image1" src="http://zh-tw.learnlayout.com/images/ilta.png"  alt=""/>
      <p class="pp1">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley o.......(略)</p>
```

完整範例

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .image-container{
          max-width: 60%;

          margin: 0 20%;
        }

        .image1{
          float: right;
        }

        .image2{
          float: left;
          margin-right: 20px;
        }

    </style>
</head>
<body>

    <div class="image-container">
      <img class="image1" src="http://zh-tw.learnlayout.com/images/ilta.png"  alt=""/>
      <p class="pp1">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum. Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.</p>

      <img class="image2" src="http://zh-tw.learnlayout.com/images/ilta.png" alt="" />
      <p class="pp2">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum. Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.</p>
    </div><!-- image-container -->

</body>
</html>
```

> 容器水平置中是使用把最大寬度設成60%，並使用`margin:auto`技巧。

###想要分段的時候用div包起來並加上`.clear-fix`

如果段落(p)高比圖片(img)小的時候會出現這樣的情況。有時候我們想要段落分明，這時候使用div包覆並幫div加上`.clear-fix`。 

![](https://lh3.googleusercontent.com/XPE7PCrLNc_xKHE_J32rggtUNIVM2G8XMJJwobHk-7AaeAYLpoLBa7YnkyXFskjfzsa1kIlDPH6Jd02Lc2H2yQ6Zlb1fbPek016YPeJ4KWTK3OnMfZalsnEr3XiWeB3AkTRAK1ycxVJOARSbfIUC09i2tqow3yBy3cHmbQ4xm_mwIbqVSbbSsbHNbNlxyLqKb5fVbI_FLyyAS8jXjB6lgjMrI0cPf2x3GCA8kR_Jzwkepluy0I5gqc-LSlJe55G5f9fLmBoHr_4wN_X212TNKYB-wCKEAN7pQUXsl8nf7N3KG0Zso20TJ2lAOgZDVHOtZzLCUpsANVDVs-KpdA8GPBuskGLGz9aVd-XRZKCvM0yI3QZPHG6-3l9GaOsfF35ALn0gv20duMZvnPyHQqgCvAhzwVgWig5c2J77quPhxwZ4eq6TOYPC7e1xrRbPhfBRlYG14AMuDmfHTHjkA7ltt-Rn2D_irzzHZ-jYw3lhSa6y74TVgTukijsnt72QNckgBOy4_82VF_4YmVZFjbxTrPPrQ2EIfy2z7a8TLrU2BKI=w1199-h542-no)

加完之後的效果，圖片一已經展開了一個長方形的空間。

![](https://lh3.googleusercontent.com/PBecy4_K22FloAL4wjCo4L921QvxF_7n8_d1t0DNaTi_CspASA_w1pQ2mFC9yr2h4PAEFsvcuQa2WNnUSLaF_K2SHYBALBl27pQiGKqdC88EsY-nrTgnEpaP2th-n-IY1N82gMbhJ6WOJwVk_oUjO7w7EA9yIyOBv0v-QVpF_7d2vj-VrwzBbxDyiu35ysSYiMxZxvY-saN0y2k9SgdNHUPGfTVsndFt-4iy5UaWgYEtxi28YtpSAeZXUov-Q27Biz5UYpwoLZutWYPet6cDmst3WMkHsouOcNPZSQR6qL2FSNIPOkYJkp5NaaKXSPiGEtg7asloDHGNMwOwht1Sz_oTznA53atrf3M36gKslKcxl71iercxW9Ip7nv9o7NacXxm-ycAXDuNZf4synvCvZhstX21YUUgaBZqSxK3mF4RXH2MI5lqa4Ml1XQOMrEKGcOXaLSqmMIZLnyj3utqpcWzk8WksidXSpaFX_Hw0m8w_FhJ73A0qEb1DTb-Cfbg4p73n3rFkG73boH0GnpKAansWNDWDf_nwc7l0bZN0Gs=w1471-h624-no)

完整範例

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .image-container{
          max-width: 60%;

          margin: 0 auto;
        }

        .image1{
          float: right;
          clear:right;
        }

        .image2{
          float: left;

          margin-right: 20px;
        }

        .image1-wrapper{
          border: 3px dashed black;
        }

        .clear-fix{
          overflow: auto;
          zoom: 1;
        }

        @media screen and (max-width: 767px) {
          .image-container {
            background-color: lightgreen;
            max-width: 100%;
          }
        }
    </style>
    <link rel="stylesheet" type="text/css" href="styke.css" media="all">
</head>
<body>
    <div class="image-container">
      <div class="clear-fix image1-wrapper">
        <img class="image1" src="http://zh-tw.learnlayout.com/images/ilta.png"  alt=""/>
        <p class="pp1">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type</p>
        <p class="pp1">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type</p>
      </div><!-- clear-fix -->
      <img class="image2" src="http://zh-tw.learnlayout.com/images/ilta.png" alt="" />
    </div><!-- image-container -->

</body>
</html>

```

###幫段落左邊加上斷行效果用`clear:left`

這次段落和img的html是：

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .image-container{
          max-width: 60%;

          margin: 0 auto;
        }

        .image1{
          float: right;
          clear:right;
        }

        .image2{
          float: left;

          margin-right: 20px;
        }

        .image1-wrapper{
          border: 3px dashed black;
        }

        .clear-fix{
          overflow: auto;
          zoom: 1;
        }



        @media screen and (max-width: 767px) {
          .image-container {
            background-color: lightgreen;
            max-width: 100%;
          }
        }
    </style>
    <link rel="stylesheet" type="text/css" href="styke.css" media="all">
</head>
<body>
  <div class="image-container">
    <img class="image2" src="http://zh-tw.learnlayout.com/images/ilta.png" alt="" />
    <p class="pp2">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type</p>
  </div><!-- image-container -->
</body>
</html>
```

呈現的效果：

![](https://lh3.googleusercontent.com/s51Inw1zkH1TZuCWGTukzpBMZZzJiw14R0xlrnY1Z0CAL03O59ukaPszTltWQlTpV_dVi_88FDMb6Hpb2h0cZ_VEtjZqcRkRosNQMYH0XCV0ABIy49LwXr4L8y6jsuv1klTh0nX9dG4FRkEew5R7ACgBBWDgKN3eNSD1kBeN90ORWraxJRGT0fihrYF7-8wKPS40VAGU3yk5KTBbfsj3zSs_chipiKSndIxPprRmdHHzvp1iHCQl5uNT_53l5mmJSquzP_fTCJ8TVvCLdLfkdGeC_waeviW30Ar9qUUXexLBfZ-nmaVXOhl7ILtYYQs4tdAGf8oyDV9-c0a8LLcupnV9qZ8qGIvafK2of3vIicEbHOJJwL1ppDhqCaZU720dyZebh2l9yJ-ukWu4ZOq5CFST7bawCSYxWVb81tePxckEogsgo0LQkHF9CcZCLLkLl2MKbBcWUVKcnqrNUnMLCX_9ZhyjVxpREXZ1HLcBCCZDyF8mi6ArIz3SXXqQqnuMnIr4sJOJ87V7HEOiNMVJkCj3YgvpKmhgDfnYsarqMdN8eqjZYz1J7lW-A5LN_HRG8wqKZJwWGqfR3SqcRsP4xIV-0pMfLcb7nD-r6Q_duU8E10s9=w1229-h349-no)


幫段落(p)加上`clear:left`後：

![](https://lh3.googleusercontent.com/q8lUS9aZ57kO_SkLiCcv7BvSwTin-tPY0BybMYnYeHikLRWNzkBlsGPDkKW3r50N0Z6kV8slZ3_aqnd8VqYBJY81N-ATQ-TQfovXZeykXSfQEBocJFWq_LFHBNyUahj5qrZo69lS0JEeBdi7cybLw6fuvVKVu_oJTuX9WGzLw4bcoH_SI0dmkolr0CD1bCRwDtRBkZvKpi0aPtnebBK1AdP8F7Cgbi0O09LA02yh_jFPdT9p326oUIkjf-8hii2ovQ7BN86JDd_UvWc5YXZBNbFOOPxix0rDBzjst6ajzXJt9vujVx4IL40Mse_0WGC_XLyv3dmfk8lp_9xGAJ2ClFDOgz4iIwWaKeMBlVU-0Sb7cMAST-6O4R_uPwqKPILF05geZ9wG6PQb-hOq3MDiRJrg6ArksK5Ow0BLX2ANbu90HR_ogG0nbl99qDi9aFc7sMjoOD3iVfGOOuh6RqVoiVbkIh4NeEUjubMr9Sm0hFYOI3hQQ4462S50h_znvrP-DEYg-Qoxa-K_CtOp1A1juRe8eKkIjEyrCZgE1qCBQ64=w1164-h391-no)


程式碼

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .image-container{
          max-width: 60%;

          margin: 0 auto;
        }

        .image1{
          float: right;
        }

        .image2{
          float: left;
          margin-right: 20px;
        }

        .image1-wrapper{
          border: 3px dashed black;
        }

        .clear-fix{
          overflow: auto;
          zoom: 1;
        }

        .pp2{
          clear: left;
        }



        @media screen and (max-width: 767px) {
          .image-container {
            background-color: lightgreen;
            max-width: 100%;
          }
        }
    </style>
    <link rel="stylesheet" type="text/css" href="styke.css" media="all">
</head>
<body>
  <div class="image-container">
    <img class="image2" src="http://zh-tw.learnlayout.com/images/ilta.png" alt="" />
    <p class="pp2">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type</p>
  </div><!-- image-container -->
</body>
</html>

```


###結論

1. 加上 float 屬性之後在`空間會受到外部 .image-wrapper 的限制`。
1. 然而對同層元素來說，設成 float 的 img 空間會消失，因此在`float img 下方的同層元素會直接浮上來`，空間跟下方的同層元素共用，內容卻是流動的，形成文繞圖的效果。
1. 如果想要斷行在 p 的左方幫p加上`float:left`，想要斷行在 p 右方幫 p 加上 `float:right`。
1. `.image-wrapper`高度比圖片的時候需要加上`clear-fix`。


####參考資料

[float與clear用法 - 布魯克斯- 點部落](http://www.dotblogs.com.tw/brooke/archive/2015/04/21/151108.aspx)
[CSS - 關於 float 屬性](http://zh-tw.learnlayout.com/float.html)




