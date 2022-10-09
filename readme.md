## corne split keyboard 藍牙版
* 燒錄檔 hex/zmk.uf2 只有右手並作為主鍵盤，以下說明有左右手配對忽略即可
* 因為規格設定為不使用OLED、RGB LED，所以不使用原版的corne的config檔，鍵盤名稱也改名為cornea
* 藍牙模組使用nrfMicro 1.4替代原設計的Arduino promicro
* 電池使用110mAh，301230鋰電池，凱旋電子供應

#### 以下為量測耗電數據的計算
  * 待機未打字狀態0.59mA 。
  * 打字狀態0.76mA，110mAh/0.76/24=21天，24小時持續打字可以連用6天。
  * 15分未使用鐘進入睡眠模式，
  * 睡眠狀態0.048mA，110mAh/0.048/24=95天，睡眠狀態可以放將近3個月。
  * 但這是理想狀態，電池品質、環境因素都會造成影響，所以計算數據僅供參考。
  * (由於鋰電池的特性，不建議用到沒電才充電，盡量維持在電量在50%以上可以延長電池壽命)
  * 從沒電充到滿電只需要1個小時(慢充也是為了增加電池壽命)
  * 沒有電源開關，帶著移動時容易誤觸喚醒，電力很容易耗光，沒有防護措施建議不要攜出使用。
  
## 操作方式
* 左右手燒好各自的燒錄檔後(就是出廠設定)可以兩邊同時按Reset鍵，讓兩邊鍵盤自動配對。

  正常情況下左右邊配對好就會一直保持著，除非刻意去改。
  
* 不使用TRRS線的設計，即使是USB有線模式，左右手鍵盤通訊還是使用無線。  

* 左邊鍵盤為主鍵盤，只有插左邊USB電腦端才會辨識為鍵盤。

* 左邊鍵盤插著USB即為USB模式，USB斷開即為藍牙模式。

* 可先使用USB模式測試左右鍵盤是否有配對成功。

* 藍牙模式與一般藍牙鍵盤配對使用方式無異。

#### 藍牙配對後如果左手有接USB充電，那會變成走USB模式，此時藍牙雖連線著但是不會進行藍牙傳輸，

#### 所以藍牙配對的電腦都沒有辦法輸入，此時把USB充電移除就藍牙可以傳輸了。

## 藍牙配對切換設定簡易說明
![image](https://github.com/ouser555/daochoc/blob/main/pic/layerdefault.png)
![image](https://github.com/ouser555/daochoc/blob/main/pic/layerlower.png)
![image](https://github.com/ouser555/daochoc/blob/main/pic/layerraise.png)
![image](https://github.com/ouser555/daochoc/blob/main/pic/layeradjust.png)
  * 此鍵盤預設有四層，default/lower/raise/adjust
  * 右手鍵盤靠拇指的下方三個鍵，為方便說明先稱作，左/中/右。想成是筆電的FN鍵就很好理解。
  * 正常情形下都在DEFAULT層。
  * 按住左鍵可以進入LOWER層。放開就回DEFAULT層。單擊一下則是ESC鍵。
  * 按住右鍵可以進入RAISE層。放開就回DEFAULT層。單擊一下則是DEL鍵。
  * 先按住左鍵即進入LOWER層，不放開，接著按右鍵，此時就是LOWER層的右鍵，這個鍵也是設為切層鍵會切到ADJUST鍵。
  * 
  * 左手鍵盤ADJUST層，由上面數來第二排都是設定成BLUE TOOTH的功能鍵。預設可以設定五組配對。
    * 由左往右
    1. Reset
    2. 清除當下這組的配對設定
    3. 切到第0組
    4. 切到第1組
    5. 切到第2組
    6. 切到第3組
    7. 切到第4組
    8. 右手
    9. 切到第4組
    10. 切到第3組
    11. 切到第2組
    12. 切到第1組
    13. 切到第0組
    14. 清除當下這組的配對設定
    15. Reset
  * 所以有時候當下的組別與電腦端無法配對時，就可以利用功能鍵清除配對設定，然後就可以重新配對了。
  * 鍵位詳情看keymap檔就可以知道了。

## 更改按鍵映射功能
  * 目前沒有更改keycode的user interface，只能靠更改程式碼來改變鍵碼。
  
  * 可以用文字編輯器打開 /cornea/cornea.keymap 修改需要的鍵碼，然後進行編譯燒錄。
  
  * https://github.com/ouser555/daochoc/blob/main/daochoc/daochoc.keymap

## 編譯方式
 
* 1. 先建立ZMK開發環境
  * 在linux terminal底下，依照官網的說明無腦複製貼上執行幾乎可以完成所有步驟
  * https://zmk.dev/docs/development/setup
  * 如果是使用Windows系統，選擇Windows菜單即是Windows的說明，
  * Windows編譯方式沒實際試過，但應該是不會有問題才是。
  
  
* 2. 將cornea資料夾
  * https://github.com/ouser555/corne_ble/tree/main/cornea
  * 放到ZMK/app/boards/shields/資料夾底下
  
  
  * 此時可以修改自己需要的 cornea.keymap
  
  
  * 用terminal進到zmk/app/ 後執行
  
    * west build -d build/left -b nrfmicro_13 -- -DSHIELD=cornea_left
    * west build -d build/right -b nrfmicro_13 -- -DSHIELD=cornea_right
  
  
  * 成功後燒錄檔會放在
    * /zmk/app/build/left/zephyr/zmk.uf2
    * /zmk/app/build/right/zephyr/zmk.uf2


   ### 補充注意事項
  * 編譯已編譯過的鍵盤
    * 之前編譯過的鍵盤，會有一些檔案產生，並且被保留下來。
    * 這些檔案有時會直接套用在新編譯的鍵盤上，造成有些修改沒有生效，
    * 所以一般建議編譯時加上-p這個參數，在編譯時可以先清除掉這些檔案。
      
    * west build -p -d build/left -b nrfmicro_13 -- -DSHIELD=cornea_left
    * west build -p -d build/right -b nrfmicro_13 -- -DSHIELD=cornea_right


## 燒錄方式

* firmware資料夾內有left、right兩個資料夾，放左右手的燒錄檔(zmk.uf2)，
  左鍵點擊進入後，右上方有download的按鈕，點擊下載。
  
* 鍵盤連接電腦USB後，連按兩下鍵盤側面的Reset鍵(電源開關旁邊)進入bootloader mode，

  成功會持續閃綠燈，電腦會把鍵盤辨識為一個隨身碟，
  
  從我的電腦打開這個隨身碟，把zmk.uf2複製進去，
  
  複製完即是燒錄完成，隨身碟會自動退出。
  
* 如果上述燒錄步驟正確的話，鍵盤為新的狀態，但之前已配對的資料還是會保留。

## 常見問題
* Q1:連按兩下Reset鍵進入bootloader模式，綠燈有閃爍，但是沒有辨識成隨身碟。

  A:應該是電腦驅動程式跑掉了。
  * 解決方式:
    1. 換USB孔試試看。
    2. 裝置管理員找到該裝置，重新安裝驅動。
    3. 重新開機。


* Q2:無法和主機成功配對

  A:應該是配對資料錯亂了，或是目前頻道已有其他配對資料。
  * 解決方式1:
  
    * 找到keymap裡面有沒有按鍵被設為 &bt BT_CLR
    * 按這個鍵可以清除目前鍵盤頻道的配對資料，然後就可以重新配對。
    
  * 解決方式2:
    * 到firmware/nrfmicro_13-settings_reset-zmk/底下，zmk.uf2燒錄檔，一樣點擊download下載。
    
      進入bootloader模式，把這個檔案燒進去，重置配對資訊。
      
      
    * 然後再重燒一次左右手的daochoc燒錄檔。
    
    
    * 兩邊燒完後同時按Reset鍵，左右手鍵盤連接後，再與電腦配對應該就可以成功配對。

* Q3:左右邊沒有辦法成功配對

  A:應該是配對資料錯亂了，或是目前頻道已有其他配對資料。

  * 解決方式:
    * 到firmware/nrfmicro_13-settings_reset-zmk/底下，zmk.uf2燒錄檔，一樣點擊download下載。
    
      進入bootloader模式，把這個檔案燒進去，重置配對資訊。
      
      
    * 然後再重燒一次左右手的ergodash燒錄檔。
    
    
    * 兩邊燒完後同時按Reset鍵，應該就可以重新配對。
