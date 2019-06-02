# LINE Notify 訊息通知
###### tags: `JavaScript` `Line`
>[自建 LINE Notify 訊息通知](https://www.oxxostudio.tw/articles/201806/line-notify.html)
>[LINE Notify 的網站](https://notify-bot.line.me/zh_TW/)
---
# 一、申請 LINE Notify 權杖

#### 首先先到LINE Notify 的網站使用自己的 LINE 帳號登入。登入後滑鼠移至上方個人帳號，選擇「個人頁面」。
![](https://i.imgur.com/stGsNDL.png)
#### 在個人頁面可以發行「權杖」，點選「發行權杖」
![](https://i.imgur.com/MMgdf4C.png)
#### 指定權杖名稱 ( 傳送通知訊息時所顯示的名稱 )，以及選擇是要一對一接收，或是讓群組也可以接收通知。
![](https://i.imgur.com/zjtBqYP.png)
#### 點選「發行」，會出現一段權杖代碼，這段代碼「只會出現一次」，複製這段代碼，先找個地方貼上並儲存這段代碼，就可以點選下方按鈕「關閉」。
![](https://i.imgur.com/hpFBl8z.png)
# 二、到google雲端使用 Google Apps Script 傳送通知
#### 由於 LINE Notify 無法直接透過「純」網頁前端的方式來發送訊息，所以必須在借重 Google Apps Script 簡易後端的能力來傳送通知，進入 Google 雲端硬碟，新增一個 Google Apps Script。
![](https://i.imgur.com/WXVs7N3.png)

#### 重新命名 Apps Script 專案名稱，並將 myFunction 函式名稱改為 doPost 或 doGet 
```javascript=
function doPost() {
    UrlFetchApp.fetch('https://notify-api.line.me/api/notify', {
        'headers': {
           'Authorization': 'Bearer ' + '你的權杖',
        },
        'method': 'post',
        'payload': {
            'message':'測試一下！'
        }
    });
}
```
#### 點選上方三角形的執行鈕，因為我們的 Apps Script 會連到外部服務，所以一開始會跳出存取權限的視窗，換句話說也就是同意外部服務使用你 Google 的帳號執行這個 Apps Script，這裡點選允許就行了。允許後，再點選一次上方三角形的執行鈕，應該就會收到 LINE Notify 發出的訊息囉。

# 三、網頁傳送 LINE Notify 訊息
#### 如果要透過網頁傳送訊息，需要改寫 Apps Script的程式，一開始增加一個 e 的參數，裡頭包含要傳送出去的 msg 訊息。
![](https://i.imgur.com/RD1fykk.png)
#### 修改程式之後，點選上方選單「發佈」，將 Apps Script 部署為網路應用程式。
![](https://i.imgur.com/5JOep7K.png)
#### 每次有修改的部署都選擇「新增」，將應用程式執行為「我」，具有應用程式存取權的使用者選擇「任何人」。
![](https://i.imgur.com/DrMXRci.png)
#### 部署後就會得到這個應用程式的網址，這個網址待會會用到。
![](https://i.imgur.com/gHXK7FP.png)
#### 完成後到網頁編輯器撰寫，先載入 jQuery，就能使用.post來發訊息，就可以送出囉！
```javascript=
function mySubmit(){
        var myMessage = document.getElementById('myLine').value;
        console.log(myMessage);
        let url = 'https://script.google.com/macros/s/AKfycbyorkTUIAkCiVLZBkmbfbsWxVknArSO_HQNNBgo0nYc6GAqQzU/exec'  //johnny
        // let url = 'https://script.google.com/macros/s/AKfycbwJ8N0jQiBzVqAFCLW4smxce0BqjHTgt_zOXTxfh_hlB96I3X4C/exec'  //david
        $.post(url,
            {msg:myMessage}
        )
        
    }
```