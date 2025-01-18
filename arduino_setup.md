
## Install Java for ARM64 & Do serial communication settings
```bash
chat@chat-desktop:~$sudo apt update
chat@chat-desktop:~$sudo apt install openjdk-8-jdk
chat@chat-desktop:~$ wget https://downloads.arduino.cc/arduino-1.8.19-linuxaarch64.tar.xz
chat@chat-desktop:~$ tar -xf arduino-1.8.19-linuxaarch64.tar.xz
chat@chat-desktop:~$ cd arduino-1.8.19
chat@chat-desktop:~/arduino-1.8.19$ sudo ./install.sh
chat@chat-desktop:~/arduino-1.8.19$ sudo usermod -a -G dialout chat
chat@chat-desktop:~/arduino-1.8.19$ ls /dev/tty*
chat@chat-desktop:~/arduino-1.8.19$ sudo chmod a+rw /dev/ttyACM0  
chat@chat-desktop:~/arduino-1.8.19$ cd 
chat@chat-desktop:~$ arduino
```

## Install SimpleDHT11 library 

## Sample code 
```c
#include <SimpleDHT.h>
int pinDHT11 = 2;
SimpleDHT11 dht11(pinDHT11);

void setup() {
  Serial.begin(9600);
}

void loop() {
  byte temperature = 0;
  byte humidity = 0;
  
  if (dht11.read(&temperature, &humidity, NULL) == SimpleDHTErrSuccess) {
    int temp = (int)temperature;
    int humid = (int)humidity;
    
    // 유효한 범위인지 확인
    if (temp >= 0 && temp <= 50 && humid >= 0 && humid <= 100) {
      Serial.print(temp);
      Serial.print(",");
      Serial.println(humid);
    }
  }
  
  delay(2000);  // 2초 대기
}

```
## Wiring

![](wiring.png)

## Install Python 3.8 on Jetson Nano
```bash
1.update & upgrade
sudo apt update-> 건너띈다.
sudo apt upgrade

2. 필요한 패키지 설치
Y


3. python3.8 소스코드 받기
cd /
sudo wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tar.xz

4. 압축 풀기
sudo tar -xf Python-3.8.12.tar.xz
cd Python-3.8.12

5. Build
#./configure --enable-optimizations
./configure --enable-loadable-sqlite-extensions --with-bz2
make -j4
6. 마무리
sudo make altinstall
python3.8 --version

7. 가상환경 (중요!!)
python3.8 -m venv myenv                                     
source myenv/bin/activate
```

## Jetson nano에 jupyter notebook 설치하기

### Running a virtual environment
```bash
source myenv/bin/activate
```

### Install Jupyter Notebook 
```bash
pip install jupyter—>  error ->sudo apt-get update
sudo apt-get install python3-dev libssl-dev libffi-dev
sudo apt-get install python3 python3-pip
pip install openai
pip install gradio

```

#### Troubleshooting

error  

export LD_LIBRARY_PATH=/usr/lib/aarch64-linux-gnu:$LD_LIBRARY_PATH


SS
(myenv) chat@chat-desktop:~/Python-3.8.12$ pip install jupyter
WARNING: pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.")': /simple/jupyter/
WARNING: Retrying (Retry(total=3, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.")': /simple/jupyter/
WARNING: Retrying (Retry(total=2, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.")': /simple/jupyter/
WARNING: Retrying (Retry(total=1, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.")': /simple/jupyter/

이 오류는 Python에서 SSL 모듈이 사용할 수 없어서 발생하는 문제입니다. pip가 패키지를 안전하게 다운로드하기 위해 HTTPS를 사용하려고 하지만, SSL 지원이 없어서 실패하고 있습니다.

이 문제를 해결하기 위한 방법들을 알려드리겠습니다:

1. SSL 지원을 위한 필수 패키지들을 설치하세요:
```bash
sudo apt-get update
sudo apt-get install python3-dev libssl-dev libffi-dev
```

2. Python을 재설치하세요. SSL 지원을 포함하여 설치하면 됩니다:
```bash
sudo apt-get install python3 python3-pip
```

3. 만약 Python을 소스에서 직접 컴파일하여 설치했다면, SSL 지원을 포함하여 다시 컴파일하세요:

./configure --with-ssl
make
sudo make install


4. 임시 해결책으로, pip 설치 시 `--trusted-host` 옵션을 사용할 수 있습니다:

pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org jupyter


하지만 보안을 위해 가능하면 SSL을 제대로 설정하는 것을 추천드립니다. SSL 없이 패키지를 설치하면 보안에 취약할 수 있습니다.

위의 방법들을 순서대로 시도해보시고, 계속 문제가 있으시다면 어떤 오류가 발생하는지 알려주시면 추가적인 도움을 드리도록 하겠습니다.



====================================
젯슨 나노 4GB에서 가상환경에 Jupyter를 설치하고 SSL 에러를 방지하는 방법은 다음과 같습니다:

1. 가상환경 생성 및 활성화:
python3.8 -m venv myenv                                     
source myenv/bin/activate

2. Jupyter 설치:
```bash
pip install jupyter notebook jupyterlab
```

3. SSL 에러 방지를 위한 OpenSSL 설치:
```bash
sudo apt update
sudo apt install openssl libssl-dev
```

4. 환경변수 설정:
```bash
export LD_LIBRARY_PATH=/usr/lib/aarch64-linux-gnu:$LD_LIBRARY_PATH
```

5. Jupyter 실행:
```bash
jupyter notebook
```

SSL 에러가 계속 발생한다면, 다음 명령어를 시도해 보세요:
```bash
jupyter notebook --allow-root --no-browser --ip=0.0.0.0
```

이 방법으로 대부분의 SSL 관련 문제를 해결할 수 있습니다[1][4]. 주의할 점은 젯슨 나노의 제한된 리소스로 인해 Jupyter Lab보다는 Jupyter Notebook을 사용하는 것이 더 안정적일 수 있습니다[6].

===========================
이 오류는 Python에서 SSL 모듈이 사용할 수 없어서 발생하는 문제입니다. pip가 패키지를 안전하게 다운로드하기 위해 HTTPS를 사용하려고 하지만, SSL 지원이 없어서 실패하고 있습니다.

이 문제를 해결하기 위한 방법들을 알려드리겠습니다:

1. SSL 지원을 위한 필수 패키지들을 설치하세요:

sudo apt-get install python3-dev libssl-dev libffi-dev

2. Python을 재설치하세요. SSL 지원을 포함하여 설치하면 됩니다:

sudo apt-get install python3 python3-pip


3. 만약 Python을 소스에서 직접 컴파일하여 설치했다면, SSL 지원을 포함하여 다시 컴파일하세요:
```bash
./configure --with-ssl
make
sudo make install
```



(myenv) chat@chat-desktop:~$ jupyter notebook
Traceback (most recent call last):
  File "/home/chat/Python-3.8.12/myenv/lib/python3.8/site-packages/jupyter_server/services/sessions/sessionmanager.py", line 14, in <module>
    import sqlite3
  File "/usr/local/lib/python3.8/sqlite3/__init__.py", line 23, in <module>
    from sqlite3.dbapi2 import *
  File "/usr/local/lib/python3.8/sqlite3/dbapi2.py", line 27, in <module>
    from _sqlite3 import *
ModuleNotFoundError: No module named '_sqlite3'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/chat/Python-3.8.12/myenv/bin/jupyter-notebook", line 5, in <module>
    from notebook.app import main
  File "/home/chat/Python-3.8.12/myenv/lib/python3.8/site-packages/notebook/app.py", line 17, in <module>
    from jupyter_server.serverapp import flags
  File "/home/chat/Python-3.8.12/myenv/lib/python3.8/site-packages/jupyter_server/serverapp.py", line 107, in <module>
    from jupyter_server.gateway.managers import (
  File "/home/chat/Python-3.8.12/myenv/lib/python3.8/site-packages/jupyter_server/gateway/managers.py", line 32, in <module>
    from ..services.sessions.sessionmanager import SessionManager
  File "/home/chat/Python-3.8.12/myenv/lib/python3.8/site-packages/jupyter_server/services/sessions/sessionmanager.py", line 17, in <module>
    from pysqlite2 import dbapi2 as sqlite3  # type:ignore[no-redef]
ModuleNotFoundError: No module named 'pysqlite2'


이 오류는 Python이 SQLite 지원 없이 컴파일되었기 때문에 발생합니다. Jupyter Notebook이 SQLite를 필요로 하는데, 필요한 모듈이 설치되지 않은 상태입니다.

이 문제를 해결하기 위해서는 다음 단계를 따라하세요:

1. 먼저 SQLite 개발 라이브러리를 설치합니다:
```bash
sudo apt-get update
sudo apt-get install sqlite3 libsqlite3-dev

```
2. Python을 재컴파일해야 합니다. 현재 디렉토리에서:
```bash
cd Python-3.8.12
./configure --enable-loadable-sqlite-extensions
make
sudo make install
```

3. 가상환경을 다시 만듭니다:
```bash
rm -rf myenv
python3 -m venv myenv
source myenv/bin/activate
```

4. 필요한 패키지들을 다시 설치합니다:
```bash
pip install jupyter notebook
```

이렇게 하면 SQLite 지원이 포함된 Python으로 Jupyter Notebook을 실행할 수 있을 것입니다. 모든 단계를 완료한 후 다시 `jupyter notebook` 명령을 실행해보세요.


ModuleNotFoundError: No module named '_bz2'
이 오류는 Python이 bz2 압축 라이브러리 지원 없이 컴파일되었기 때문에 발생합니다. 해결을 위해 다음 단계를 따라해주세요:

1. 먼저 가상환경을 빠져나옵니다:
```bash
deactivate
```

2. bz2 개발 라이브러리를 설치합니다:
```bash
sudo apt-get install libbz2-dev
```

3. Python을 다시 컴파일합니다:
```bash
cd Python-3.8.12
./configure --enable-loadable-sqlite-extensions --with-bz2
make
sudo make install
```

4. 가상환경을 새로 만듭니다:
```bash
rm -rf myenv
python3 -m venv myenv
source myenv/bin/activate
```

5. 필요한 패키지들을 다시 설치합니다:
```bash
pip install jupyter notebook gradio pandas ipykernel
```

6. 가상환경을 Jupyter kernel로 등록합니다:
```bash
python -m ipykernel install --user --name=myenv --display-name="Python (myenv)"
```

이렇게 하면 bz2 지원이 포함된 Python으로 모든 패키지가 정상적으로 작동할 것입니다. Jupyter Notebook을 다시 시작하고 새로 등록한 kernel을 선택해서 사용해보세요.


