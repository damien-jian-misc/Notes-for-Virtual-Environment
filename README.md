# Python Virtual Environment 虛擬環境的增刪查改

這份文件旨在說明`pyenv`、`virtualenv`以及`pip3`的功能與使用方法。假設系統為`Mac OS`；`Terminal`的`Shell`語言為`zsh`或`bash`。

參考資料：

<https://ithelp.ithome.com.tw/articles/10339000>

## `pyenv`：建立不同`Python`版本

`pyenv`及`conda`都可以用來建立不同`Python`版本。這裡選擇介紹`pyenv`。

### 安裝並設定`pyenv`

1. 安裝`pyenv`
   假設`Mac`系統中已裝有`Homebrew`，則可以使用`Homebrew`來安裝`pyenv`。開啟`Terminal`，輸入並執行（按下`Enter`鍵）以下這行命令，即可安裝`pyenv`。

    ```bash
    brew install pyenv
    ```

    安裝完成後，按照`Homebrew`的輸出指示，將下列代碼添加到`shell`啟動文件中。這通常是`.bashrc`或`.zshrc`。我的系統的`shell`是`zsh`語法。(Q: 到底加在`~/.zshrc`還是`~/.zprofile`尚存疑。)

    ```bash
    if command -v pyenv 1>/dev/null 2>&1; then
    eval "$(pyenv init --path)"
    fi
    ```

2. 重新啟動`Terminal`，或者（在`Terminal`中）執行以下命令來使配置生效：

    ```bash
    source ~/.zshrc
    ```

    或

    ```bash
    source ~/.bashrc
    ```

### 安裝`Python`版本

在`Terminal`中使用`pyenv install`命令安裝想要的`Python`版本，例如`3.9.18`：

```bash
pyenv install 3.9.18
```

安裝完成後，可以（在`Terminal`中）使用`pyenv versions`查看已安裝的`Python`版本。

```bash
pyenv versions
```

`Terminal`應該會返回

```bash
  system
* 3.9.18 (set by /Users/damien_jian/.pyenv/version)
```

或

```bash
* system
  3.9.18 (set by /Users/damien_jian/.pyenv/version)
```

### 設置`Python`版本

（在`Terminal`中）使用`pyenv global`設置全局`Python`版本，例如：

```bash
pyenv global 3.9.18
```

這將設置全局`Python`版本為`3.9.18`。

- `global`這個字可有三種選項可以替換：
  - `global`：全局
  - `local`：本資料夾
  - `shell`：本`shell`視窗
- 版本號 `3.9.18`則可以系統已安裝的版本替換：
  - `3.9.18`
  - `system`
  - ...

切換完成後，可以（在`Terminal`中）使用`python --version`查看目前的`Python`版本。

```bash
python3 --version
```

## `virtualenv`：建立相互獨立的`Python`虛擬環境

`virtualenv`及`venv`都可以用來建立`Python`虛擬環境。

`virtualenv`(`pyenv-virtualenv`)是`pyenv`的一個插件。用`virtualenv`來建立的`Python`虛擬環境會與用`pyenv`建立的各種`Python`版本在同一個資料夾中，所以很方便統一管理各版本們及各版本的各虛擬環境們。

`venv`是`Python`中的一個套件，因此`venv`只能在當前`Python`版本裡建立虛擬環境。如果有需要在不同`Python`版本之間切換的需求時，不適合用`venv`建立虛擬環境。

以下先介紹`virtualenv`(`pyenv-virtualenv`)。

### `virtualenv`（`pyenv-virtualenv`）使用介紹

#### 安裝`pyenv-virtualenv`

在`Terminal`中輸入並執行

```zsh
brew install pyenv-virtualenv
```

安裝完成後在shell配置文件(例如`~/.bashrc`、`~/.zshrc`、`~/.bash_profile`、`~/.zprofile`等)（我建議加在`~/.bash_profile`或`~/.zprofile`）中添加初始化`pyenv-virtualenv`的代碼。因為我用`zsh`，所以我加在`~/.zprofile`

```zsh
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
```

#### 創建虛擬環境

確認已經安裝所需的`Python`版本。如果未安裝，可以先用`pyenv`安裝。
假設要安裝的版本是`3.9.18`。

```zsh
pyenv install 3.9.18  # 替換為所需的版本
```

創建一個新的虛擬環境。
假設要安裝的版本是`3.9.18`。假設欲將虛擬環境取名為`3.9.18-my-project-env`。

```zsh
pyenv virtualenv 3.9.18 3.9.18-my-project-env
```

#### 刪除虛擬環境

假設要刪除的虛擬環境名稱為`3.9.18-my-project-env`。使用`pyenv virtualenv-delete`。

```zsh
pyenv virtualenv-delete 3.9.18-my-project-env
```

#### 啟用虛擬環境

```zsh
pyenv activate 3.9.18-my-project-env
```

#### 停用虛擬環境

```zsh
pyenv deactivate 3.9.18-my-project-env
```

###（目前不建議使用！！！）`venv`：建立互相獨立的`Python`虛擬環境（另一種）

目前不建議使用`venv`來建立虛擬環境。

參考資料：<https://docs.python.org/zh-tw/3/tutorial/venv.html>

假設系統已安裝`venv`。（如何安裝要再查）

#### 建立新環境

移至要建立虛擬環境的目錄：(以下為範例，具體路徑是目標目錄的路徑)

```bash
cd /Users/damien_jian/Desktop
```

在`Terminal`中輸入：（zsh 語法）

```bash
python3 -m venv .venv
```

這會在當前`directory`建立一個新的虛擬環境（一個`directory`），名字叫做`.env`。
`-m`的意思是以當前`Python`版本。

一旦建立了一個虛擬環境，你可以啟動它。

```bash
source .venv/bin/activate
```

也可以關閉它。

```bash
deactivate
```

## `pip3`：管理`Python`環境中的套件

參考資料：<https://docs.python.org/zh-tw/3/tutorial/venv.html>

`pip3`中的`3`如果省略的話，恐有跟`pip2`搞混之虞。

- 安裝一個模組：

  ```bash
  pip3 install <module>
  ```

- 將一個環境中的模組組合記錄下來，並儲存於當前目錄下的`requirement.txt`檔（若該檔案不存在則會自動創建並取名該檔案）中：

  ```bash
  python3 -m pip freeze > requirements.txt
  ```

  或

  ```bash
  pip3 freeze > requirements.txt
  ```

  目前猜測在非虛擬環境中（`system`原生環境）中執行上述命令時所生成的`requirement.txt`中會出現一些如下內容，則`pip3 install`無法解析，例如：

  ```md
  altgraph @ file:///System/Volumes/Data/SWE/Apps/DT/BuildRoots/BuildRoot7/ActiveBuildRoot/Library/Caches/com.apple.xbs/Sources/python3/python3-133.100.1.1/altgraph-0.17.2-py2.py3-none-any.whl

  future @ file:///System/Volumes/Data/SWE/Apps/DT/BuildRoots/BuildRoot7/ActiveBuildRoot/Library/Caches/com.apple.xbs/Sources/python3/python3-133.100.1.1/future-0.18.2-py3-none-any.whl

  macholib @ file:///System/Volumes/Data/SWE/Apps/DT/BuildRoots/BuildRoot7/ActiveBuildRoot/Library/Caches/com.apple.xbs/Sources/python3/python3-133.100.1.1/macholib-1.15.2-py2.py3-none-any.whl
  ```

  解決`pip3 install`無法解析上述內容的方法為：將`@`之後的文字刪掉，並且將版本參數加在模組名稱後面，例如：

  ```md
  altgraph==0.17.2
  future==0.18.2
  macholib==1.15.2
  ```

  如此一來以下的`pip3 install -r requirements.txt`才不會報錯。只要不是`freeze`系統原生`Python`那麼應該不會發生這種情況。

- 當要使用一個特定的模組組合時，可以使用以下命令：

  ```bash
  pip3 install -r requirements.txt
  ```

  或是

  ```bash
  python3 -m pip install -r requirements.txt
  ```
