# 如何使用 SSH 連線來推送代碼到 GitHub
本文將介紹如何使用 SSH 協議來與 GitHub 服務器通信，並且使用 git push 命令來推送您的代碼到 GitHub 上。SSH 協議是一種用於安全地連接到遠程服務器的協議，它可以提供高強度的安全性和高效的性能。要使用 SSH 協議，您需要生成一對 SSH 公開金鑰和私密金鑰，並將公開金鑰添加到 GitHub 上。以下是具體的步驟：
## 1. 生成 SSH 公開金鑰和私密金鑰
- 打開終端，執行以下命令，生成一對 ed25519 類型的 SSH 公開金鑰和私密金鑰：
  ```bash
  ssh-keygen -t ed25519 -C "GitHub key for user@example.com"
  ```
  請替換 `user@example.com` 為您的 GitHub 註冊郵箱。
  ***
  ### ssh-keygen
  ssh-keygen 是一個用於生成 SSH 密鑰對的工具，SSH 密鑰對可以用於 SSH 協議的身份驗證和加密。以下是用到的 ssh-keygen 引數：
  -t ed25519：這個引數指定了生成密鑰對的類型為 ed25519，這是一種基於橢圓曲線加密（ECC）的算法，它可以提供高強度的安全性和高效的性能。ed25519 是目前推薦的 SSH 密鑰類型。(據 ChatGPT)
  -C：這個引數指定了生成密鑰對時添加的註釋，這是一個可選的字符串，通常用於標識密鑰的用戶或用途。註釋不會影響密鑰的功能，但可以幫助管理和辨認密鑰。
  ***
- 按 Enter 鍵，接受默認的文件名 `/home/your_username/.ssh/id_ed25519` 和 `/home/your_username/.ssh/id_ed25519.pub` 來保存您的金鑰對。如果您想要自定義文件名，您可以輸入一個不同的路徑。
- 按 Enter 鍵，選擇不設置通行短語（passphrase）來保護您的私密金鑰。如果您想要設置通行短語，您可以輸入一個字符串，並在提示時再次輸入。如果您設置了通行短語，那麼每次使用 SSH 連接時，您都需要輸入這個通行短語。
- 等待金鑰對生成完成，並查看生成的指紋（fingerprint）和隨機藝術圖（randomart）。指紋是一個用於唯一標識和驗證金鑰的字符串，隨機藝術圖是一個用於視覺化和辨認金鑰的圖像。
## 2. 添加 SSH 公開金鑰到 GitHub 上
- 在終端中，執行以下命令，查看並複製您的公開金鑰文件的內容：
  ```bash
  cat ~/.ssh/id_ed25519.pub
  ```
  如果您自定義了文件名，請替換 `~/.ssh/id_ed25519.pub` 為實際的路徑。
- 或者是這個命令，也可以查看和管理已經加載到 SSH 代理中的公開金鑰。
  ```bash
  ssh-add -l
  ```
- 登錄到您的 GitHub 帳戶。
- 點擊右上角的頭像，選擇「Settings」（設置）。
- 在左側選單中，選擇「SSH and GPG keys」（SSH 和 GPG 密鑰）。
- 點擊「New SSH key」（新建 SSH 密鑰）。
- 在「Title」（標題）欄位中，輸入一個描述性的名稱，例如「My SSH key」。
- 在「Key」（密鑰）欄位中，粘貼您之前複製的公開金鑰文件的內容。
- 點擊「Add SSH key」（添加 SSH 密鑰）。
## 3. 配置本地 Git 存儲庫中的 SSH URL
- 打開終端，進入您的本地 Git 存儲庫所在的目錄。
- 執行以下命令，將您的 GitHub 用戶名和倉庫名配置為遠程存儲庫的 SSH URL。請替換 `YOUR_USERNAME` 為您的 GitHub 用戶名，`YOUR_REPO` 為您的 GitHub 倉庫名：
  ```bash
  git remote set-url origin git@github.com:YOUR_USERNAME/YOUR_REPO.git
  ```
  上述命令中的 origin 是您的遠程存儲庫的名稱。確保替換 YOUR_USERNAME 和 YOUR_REPO 為實際的值。
## 4. 進行遠程推送操作
- 現在，您可以使用常規的 git push 命令來推送更改到 GitHub，無需再輸入用戶名和密碼。例如：
  ```bash
  git push origin main
  ```
  此時，Git 將使用您之前生成和配置的 SSH 公開金鑰和私密金鑰來進行身份驗證。
- 如果您之前設置了通行短語，您可能需要輸入您的通行短語來解鎖您的私密金鑰。如果您想要避免每次都輸入通行短語，您可以使用 SSH 代理（ssh-agent）來記住您的通行短語。
## 5. 也可以遠端 clone github 的儲存庫
- 透過終端機將「當前目錄」改至欲存放遠端檔案的目錄下。
- 在設定好 SSH 金鑰之後，於 terminal 輸入：
  ```bash
  git clone git@github.com:username/repo.git
  ```
