---
author: StevenTTuD
layout: post
title: EDX Linux Foundation 補充筆記
published: true
date: 2014-09-30 17:18
tags:
  - Linux
  - EDX Linux Foundation
comments: true

---

1. 七層筆記＋tcp/ip筆記
2. ip
3. mac
4. socket
5. 基本知識
乙太網路
了解封包內容


##名詞釐清
IEEE
國際上專業制定單位的組織
LAN (LOCAL AREA NETWORK)
區域網路
WAN (WIDE  AREA NETWORK)
廣域網路
NIC(NETWORK INTERFACE CARD)
NI


#1. OSI (Open System Interconnection) 七層協定
##layer 1 phisical layer
##layer 2 data link layer
###偏軟體的子層 MAC(media access control)
MAC 是網路媒體所能處理的主要資料包裹，這也是最終被實體層編碼成位元串的資料。
####MAC frame
###偏硬體的子層LLC(logical link control)
主要在多工處理來自上層的封包資料 (packet) 並轉成 MAC 的格式

##layer 3 Network layer
###IP
###route

##layer 4 transport layer
##layer 5 session layer
##layer 6 presentation layer
##layer 7 application layer



#2. TCP/IP是什麼？如何運作？

##2.TCP/IP 如何運作
那 TCP/IP 是如何運作的呢？我們就拿妳常常連上的 Yahoo 入口網站來做個說明好了，整個連線的狀態可以這樣看：

1. 應用程式階段：妳打開瀏覽器，在瀏覽器上面輸入網址列，按下 [Enter]。此時網址列與相關資料會被瀏覽器包成一個資料， 並向下傳給 TCP/IP 的應用層；
2. 應用層：由應用層提供的 HTTP 通訊協定，將來自瀏覽器的資料包起來，並給予一個應用層表頭，再向傳送層丟去；
3. 傳送層：由於 HTTP 為可靠連線，因此將該資料丟入 TCP 封包內，並給予一個 TCP 封包的表頭，向網路層丟去；
4. 網路層：將 TCP 包裹包進 IP 封包內，再給予一個 IP 表頭 (主要就是來源與目標的 IP 囉)，向鏈結層丟去；
5. 鏈結層：如果使用乙太網路時，此時 IP 會依據 CSMA/CD 的標準，包裹到 MAC 訊框中，並給予 MAC 表頭，再轉成位元串後， 利用傳輸媒體傳送到遠端主機上。
等到 Yahoo 收到你的包裹後，在依據相反方向拆解開來，然後交給對應的層級進行分析，最後就讓 Yahoo 的 WWW 伺服器軟體得到你所想要的資料，該伺服器軟體再根據你的要求，取得正確的資料後，又依循上述的流程，一層一層的包裝起來， 最後傳送到你的手上！就是這樣囉！

## MAC
MAC 其實就是我們上面一直講到的訊框 (frame) 囉！ 只是這個訊框上面有兩個很重要的資料，就是目標與來源的網卡卡號，因此我們又簡稱網卡卡號為 MAC 而已。 簡單的說，你可以把MAC想成是一個在網路線上面傳遞的包裹，而這個包裹是整個網路硬體上面傳送資料的最小單位了。 也就是說，網路線可想成是一條『一次僅可通過一個人』的獨木橋， 而 MAC 就是在這個獨木橋上面動的人啦！

##乙太網路
乙太網路已經是一項公認的標準介面了，如此一來，大家都可以依據這個標準來設定與開發自己的硬體， 只要硬體符合這個標準，理論上，他就能夠加入乙太網路的世界，所以，購買乙太網路時， 僅需要查看這個乙太網路卡支援哪些標準就能夠知道這個硬體的功能有哪些， 而不必知道這個乙太網路卡是由哪家公司所製造的吶

## RJ-45接頭

###乙太網路的傳輸協定：CSMA/CD
每張乙太網路卡出廠時，就會賦予一個獨一無二的卡號，那就是所謂的 MAC (Media Access Control) 啦！

##route

#IP 的取得方式
##static
##DHCP(Dynamic Host Configuration Protocol)
##透過撥接取得：
向你的 ISP 申請註冊，取
得帳號密碼後，直接撥接到 ISP ，你的 ISP 會透過他們自己的設定，讓你的作業系統取得正確的網路參數。


IP 是門牌，TCP 是樓層，真正提供服務的，是在該樓層的那個人 (Protocol)！

##ICMP 協定
ICMP 的全名是『 Internet Control Message Protocol, 網際網路訊息控制協定 』。 基本上，ICMP 是一個錯誤偵測與回報的機制，最大的功能就是可以確保我們網路的連線狀態與連線的正確性！ ICMP 也是網路層的重要封包之一，不過，這個封包並非獨立存在，而是納入到 IP 的封包中！
