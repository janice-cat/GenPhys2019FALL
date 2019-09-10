Installation Guideline -- 09/11 class note 

# Windows
安裝詳細說明（影）：  
+ [Python](https://www.youtube.com/watch?v=ZFA7_sWln48)  
+ [VPython](https://www.youtube.com/watch?v=comdEwP7_Kc)
  
 ### Python
 + 確認32, 64 位元：  
 http://esupport.trendmicro.com/solution/zh-tw/1112433.aspx  
 + Download site (**python3.6.6**): https://www.python.org/downloads/release/python-366/ 
     1. 64 bit: Windows x86-64 web-based installer
     2. 32 bit: Windows x86 web-based installer

### VPython
 + Open cmd
    win + r
 + Go to the the python36/Scripts path
     ```bash
     cd <your python36/Scripts path>
     ```
 + Install vpython
    ```bash
    pip3 install vpython
    ```
 + Come into error
     + First try upgrading pip
        ```bash
        python -m pip install --upgrade pip
        
        pip3 install vpython
        ```
     + Upgrade setuptools
        ```bash
        pip install --upgrade setuptools
        
        pip3 install vpython
        ```  
## (Other way) Visual Studio 2017
+ Visual Studio 2017 Download Site  
https://visualstudio.microsoft.com/zh-hant/thank-you-downloading-visual-studio/?sku=Community&rel=15
+ Intall Python Package (透過圖形化介面安裝套件)  
https://docs.microsoft.com/zh-tw/visualstudio/python/tutorial-working-with-python-in-visual-studio-step-05-installing-packages?view=vs-2017

# Mac
[詳細說明](https://drive.google.com/file/d/1930IZ1eq2tHB8EvnrxYtdhntrOdlqyu9/view?usp=sharing)  
### Install xcode
```bash
xcode-select --install
```
### Install Homebrew
 + Install Homebrew
    ```bash
    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
 + Test
    ```bash
    brew doctor
    ```
### Python
 + Python3 Install
    ```bash
    brew install python3
    ```
 + Test
    ```bash
    which python3
    which pip3
    ```
    ```bash
    python3
    ```

### VPython
 + Install vpython
    ```bash
    pip3 install vpython
    ```
 + Come into error
     + First try upgrading pip
        ```bash
        python3 -m pip install --upgrade pip
        
        pip3 install vpython
        ```
     + Upgrade setuptools
        ```bash
        pip3 install --upgrade setuptools
        
        pip3 install vpython
        ```

** 若出現permission denied: 請在有問題的指令前加sudo重試該指令

# Test
```python
from vpython import *
sphere()
```

# Q&A
1. **python3.7**的問題  
    **Error Msg:**  
    > error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": http://landinghub.visualstudio.com/visual-cpp-build-tools  
    > \---------------------------------------------  
    Command "...\python\python37\python.exe -u -c"import setuptools, tokenize;\_\_file\_\_='...\\pip-install-xxx\\vpython\\setup.py' ;f=getattr(tokenize, 'open', open)(\_\_file\_\_);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, \_\_file\_\_, 'exec'))" install --record ...\pip-record-_xxx\install-record.txt --single-version-externally-managed --compile" failed with error code 1 in ...\pip-install-xxx\vpython\
    
    **Sol:**  
    請安裝python3.6.6喔：  
https://www.python.org/downloads/release/python-366/  

2. 執行`pip install vpython`時，與**vpnotebook** 有關的問題 :  
    **Error Msg:**  
    > Command ".../python3.6 -u -c "import setuptools, tokenize;\_\_file\_\_='.../pip-install-xxx/vpnotebook/setup.py';f=getattr(tokenize, 'open', open)(\_\_file\_\_);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, \_\_file\_\_, 'exec'))" install --record .../pip-record-\_xxx/install-record.txt --single-version-externally-managed --compile --install-headers .../include/site/python3.6/vpnotebook" failed with error code 1 in .../pip-install-xxx/vpnotebook/
      
    **Sol:**  
    ```bash
    python3 -m pip install --upgrade pip

    pip3 install vpython
    ```  
3. 權限問題（for windows）：  
    **Error Msg:**  
    >Could not install packages due to an EnvironmentError: [WinError 5]存取被拒。: 'c:\\program files (x86)\\microsoft visual studio\\shared\\...'  
    Consider using the \`--user\` option or check the permissions.
      
     **Sol:**  
     c:\program files (x86) 是受保護的區域，所以必須要有管理者權限才能將套件寫入。  
     請在打開cmd時，按右鍵，選取“以系統管理員身份執行”，再來執行pip command。  
