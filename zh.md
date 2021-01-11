# 單元測試的藝術 ch7
```text
7.1 執行自動化測試的自動化建置
     7.1.1 建置腳本結構
     7.1.2 觸發建置和整合
 
7.2 依據速度和種類對應的測試分類
     7.2.1 分開整合測試和單元測試的人為因素
     7.2.2 綠色安全區域
 
7.3 確保測試程式是版本庫管理的一部分

7.4 將測試類別的位置與被測試程式相對應
     7.4.1 將測試對應到專案
     7.4.2 把測試對應到類別
     7.4.3 將測試對應到明確的工作單元入口
 
7.5 注入橫切面關注點

7.6 為應用程式建立測試API
     7.6.1 使用繼承類別繼承模式
     7.6.2 建立測試輔助類別和方法
     7.6.3 把你的API 介紹給開發人員
 
7.7 小結
```

* 無論您執行什麼測試（無論您如何執行），都將其自動化，使用自動化構建過程在白天或晚上盡可能多地運行它，並***儘可能連續地交付產品***。
    > 那什麼是沒有自動化的單元測試呢？

* 將整合測試與單元測試（慢速測試與快速測試）***分開***，以便您的團隊可以擁有一個安全的綠色區域，***所有測試都必須通過***。
    > 維護綠色安全是最基本的要求。

* 按項目和類型劃分測試（單元測試與集成測試，慢速測試與快速測試），並將它們分為不同的專案，文件夾或名稱空間（或全部）。我通常使用所有三種類型的分離。
    >  透過明顯的物理區分，讓測試目標可以更明確的執行，而且耗費的心力相對少。

* 使用測試類層次結構將相同的測試集應用於層次結構中被測試的多個相關類型或共享公共接口或基類的類型。
> o

* 如果測試類的繼承使測試的可讀性降低，那就使用輔助類別(helper classes)和工具類別(utiity classes)而不是繼承結構，尤其是在基類中使用共享的setup的情況下。不同的人對何時使用哪種有不同的看法，但是可讀性通常是不使用繼承結構的主要原因。
  * Use helper classes and utility classes instead of hierarchies if the test class hierarchy makes tests less readable, especially if there’s a shared setup method in the base class. Different people have different opinions on when to use which, but readability is usually the key reason for not using hierarchies.
  > reuse 程式碼的方式有很多種，而繼承偏偏是最難回去看懂，但是很簡單就可以實作的。
    
* 讓您的團隊知道您的API。如果您不這樣做，您將浪費時間和金錢，因為團隊成員在不知不覺中一遍又一遍地重新發明了API。
    > 不只是其他團隊成員，其他專案也會有這個問題。

---


## 7.1 執行自動化測試的自動化建置 p.154
bulid process 有 
1. build scripts, 
2. bulid integaration servers, 
3. build triggers, and 
4. a share teamunderstanding and acceptance of how code is deployed and integrated.

  >   7.1.1 建置腳本結構
  > 使用多個腳本, 一個腳本完成一個功能 p.155 
* A continuous integration (CI) build script
* A nightly build script 任何不再 CI bs 裡面的任務（無關、不夠重要）
* A deployment build script

	> because I want my build script actions to be version aware (or version controlled), so I can always get back to any version of the source and my build actions will be relevant to that version.

  >   7.1.2 觸發建置和整合
CI server 主要功能包含：
* Trigger a build script based on specific events
* Provide build script context and data such as version, source code, and artifacts from other builds, build script parameters, and so on
* Provide an overview of build history and metrics
* Provide the current status of all the active and inactive builds

Bulid Configuration 有 command
Bulid Configuration 有 context 如 commit tree、Set Unix ENV、bash direct parameters、上次可重用部分

 
7.2 依據速度和種類對應的測試分類
     7.2.1 分開整合測試和單元測試的人為因素
     7.2.2 綠色安全區域
 
7.3 確保測試程式是版本庫管理的一部分

7.4 將測試類別的位置與被測試程式相對應
     7.4.1 將測試對應到專案
     7.4.2 把測試對應到類別
     7.4.3 將測試對應到明確的工作單元入口
 
7.5 注入橫切面關注點

7.6 為應用程式建立測試API
     7.6.1 使用繼承類別繼承模式
     7.6.2 建立測試輔助類別和方法
     7.6.3 把你的API 介紹給開發人員
 
7.7 小結
