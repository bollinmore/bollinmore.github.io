# 使用 Line Login 登入你的服務

## Basic concept
什麼時候會用到Line Login？如果說你沒有打算要求使用者註冊你的平台，要做到類似其他網站上面讓人用Google帳號登入的方式時就可以用此API.  
運作方式圖解：

``` mermaid
sequenceDiagram
  User->>Your_Service: Click the Line Login button.
  Your_Service->>Line_Auth: Redirect to Line Auth page.
  Line_Auth-->>Your_Service: Redirect to callback URL with code.
  Your_Service->>Line_Auth: Query user date with code.
  Line_Auth-->>Your_Service: Return payload with id token.
  Your_Service->>Line_Auth: Verify id token.
  Line_Auth-->>Your_Service: Return the credential data.
  loop Store Credential
      Your_Service->>Your_Service: Save user's data to database.
  end
  Your_Service-->>User: Login successfully!
```

## 設定流程與參數
* 在你的網頁上放置Line Login按鈕，讓使用者可以點
* 點擊之後**重導**至https://access.line.me/oauth2/v2.1/authorize 並加上以下參數
    * response_type：code 會回傳參數名 code 的授權碼
    * client_id：你的ChannelID
    * redirect_uri： Callback URL這是在Line Developer Console中設定的
    * state：自訂驗證碼，也可以隨便填
    * scope：欲取得 User 的資料，如想取得 openid 和 profile，填寫 openid%20profile
* 接著你要用傳回來的 Code 再發一次Request https://api.line.me/oauth2/v2.1/token
    * grant_type: 預設填 `authorization_code`
    * code: 剛剛傳回來的 `code`
    * redirect_uri: 跟剛剛一樣的 Callback URL
    * client_id: 你的ChannelID
    * client_secret: 你的Secret, 在Line Developer Console可以查到
* 最後發一次 Reqest 驗證資料 https://api.line.me/oauth2/v2.1/verify
    * id_token: 前一次回傳的JSON data中的 id_token 欄位
    * client_id：你的ChannelID


## 範例
### Authorize
#### Request

#### Response
### Get Token