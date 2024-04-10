# Python Virtual Environment 虛擬環境的增刪查改

這份文件旨在說明 `pyenv`、`venv` 以及 `pip3` 的功能與使用方法。假設系統為 `Mac OS`；`Terminal` 為 `zsh` 語法。

參考資料：

<https://ithelp.ithome.com.tw/articles/10339000>

## `pyenv`：建立不同 `Python` 版本

### 安裝並設定 `pyenv`

1. 安裝 `pyenv`
   假設 `Mac` 系統中已裝有 `Homebrew` ，則可以使用 `Homebrew` 來安裝 `pyenv`。開啟 `Terminal`，輸入並執行（按下 `Enter` 鍵）以下這行命令，即可安裝 `pyenv`。

    ```bash
    brew install pyenv
    ```

    安裝完成後，按照 `Homebrew` 的輸出指示，將下列代碼添加到 `shell` 啟動文件中。這通常是 `.bashrc` 或 `.zshrc`。我的系統是的 `shell` 是 `zsh` 語法。

    ```bash
    if command -v pyenv 1>/dev/null 2>&1; then
    eval "$(pyenv init --path)"
    fi
    ```

2. 重新啟動 `Terminal` ，或者（在 `Terminal` 中）執行以下命令來使配置生效：

    ```bash
    source ~/.zshrc
    ```

    或

    ```bash
    source ~/.bashrc
    ```

### 安裝 `Python` 版本

在 `Terminal` 中使用 `pyenv install` 命令安裝想要的 `Python` 版本，例如 `3.9.18`：

```bash
pyenv install 3.9.18
```

安裝完成後，可以（在 `Terminal` 中）使用 `pyenv versions` 查看已安裝的 `Python` 版本。

```bash
pyenv versions
```

`Terminal` 應該會返回

```bash
  system
* 3.9.18 (set by /Users/damien_jian/.pyenv/version)
```

或

```bash
* system
  3.9.18 (set by /Users/damien_jian/.pyenv/version)
```

### 設置 `Python` 版本

（在 `Terminal` 中）使用 `pyenv global` 設置全局 `Python` 版本，例如：

```bash
pyenv global 3.9.18
```

這將設置全局 `Python` 版本為 `3.9.18`。

- `global` 這個字可有三種選項可以替換：
  - `global`：全局
  - `local`：本資料夾
  - `shell`：本`shell`視窗
- 版本號 `3.9.18` 則可以系統已安裝的版本替換：
  - `3.9.18`
  - `system`
  - ...

切換完成後，可以（在 `Terminal` 中）使用 `python --version` 查看目前的 `Python` 版本。

```bash
python --version
```

## `venv`：建立互相獨立的 `Python` 虛擬環境

參考資料：<https://docs.python.org/zh-tw/3/tutorial/venv.html>

假設系統已安裝 `venv`。（如何安裝要再查）

### 建立新環境

移至要建立虛擬環境的目錄：(以下為範例，具體路徑是目標目錄的路徑)

```bash
cd /Users/damien_jian/Desktop
```

在 `Terminal` 中輸入：（zsh 語法）

```bash
python3 -m venv .venv
```

這會在當前 `directory` 建立一個新的虛擬環境（一個 `directory`），名字叫做 `.env`。
`-m` 的意思是以當前 `Python` 版本。

一旦建立了一個虛擬環境，你可以啟動它。

```bash
source .venv/bin/activate
```

也可以關閉它。

```bash
deactivate
```

## `pip3`：管理 `Python` 環境中的套件

參考資料：<https://docs.python.org/zh-tw/3/tutorial/venv.html>

`pip3` 中的 `3` 如果省略的話，恐有跟 `pip2` 搞混之虞。

- 安裝一個模組：

  ```bash
  pip3 install <module>
  ```

- 將一個環境中的模組組合記錄下來，並儲存於當前目錄下的 `requirement.txt` 檔（若該檔案不存在則會自動創建並取名該檔案）中：

  ```bash
  python3 -m pip freeze > requirements.txt
  ```

  或

  ```bash
  pip3 freeze > requirement.txt
  ```

  目前猜測在非虛擬環境中（`system`原生環境）中執行上述命令時所生成的 `requirement.txt` 中會出現一些如下內容，則 `pip3 install` 無法解析，例如：

  ```md
  altgraph @ file:///System/Volumes/Data/SWE/Apps/DT/BuildRoots/BuildRoot7/ActiveBuildRoot/Library/Caches/com.apple.xbs/Sources/python3/python3-133.100.1.1/altgraph-0.17.2-py2.py3-none-any.whl

  future @ file:///System/Volumes/Data/SWE/Apps/DT/BuildRoots/BuildRoot7/ActiveBuildRoot/Library/Caches/com.apple.xbs/Sources/python3/python3-133.100.1.1/future-0.18.2-py3-none-any.whl

  macholib @ file:///System/Volumes/Data/SWE/Apps/DT/BuildRoots/BuildRoot7/ActiveBuildRoot/Library/Caches/com.apple.xbs/Sources/python3/python3-133.100.1.1/macholib-1.15.2-py2.py3-none-any.whl
  ```

  解決 `pip3 install` 無法解析上述內容的方法為：將 `@` 之後的文字刪掉，並且將版本參數加在模組名稱後面，例如：

  ```md
  altgraph==0.17.2
  future==0.18.2
  macholib==1.15.2
  ```

  如此一來以下的 `pip3 install -r requirements.txt` 才不會報錯。

- 當要使用一個特定的模組組合時，可以使用以下命令：

  ```bash
  pip3 install -r requirements.txt
  ```

  或是

  ```bash
  python3 -m pip install -r requirements.txt
  ```
