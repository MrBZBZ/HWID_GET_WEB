# HWID验证网页
* 用于软件使用HWID验证用户是否合法的网页，在网页上存储内部人员的HWID以供验证
* 仓库里的html文件仅供参考，为作者的软件验证所用
# HWID验证原理
* 程序访问已经部署好的Github Pages网页，在网页端存储好HWID信息，在本地识别到的HWID用if语句判断是否在网页上已存储，达到验证用户的目的
# 获取本地HWID的代码（python）
```python
import subprocess
import hashlib
import request
```
* 你首先需要安装以上这些库，然后在你的代码中引用，可以使用以下命令安装
```python
pip install subprocess
pip install hashlib
pip install request
```
* 接着写一个名为get_hwid的函数
```python
def get_hwid():
  CREATE_NO_WINDOW = 0x08000000
  # 获取CPU信息
  cpu = subprocess.check_output('wmic cpu get ProcessorId', shell=True, creationflags=CREATE_NO_WINDOW).decode().split('\n')[1].strip()
  # 获取硬盘信息
  HDD = subprocess.check_output('wmic diskdrive get SerialNumber', shell=True, creationflags=CREATE_NO_WINDOW).decode().split('\n')[1].strip()
  # 获取主板信息
  MB = subprocess.check_output('wmic baseboard get SerialNumber', shell=True, creationflags=CREATE_NO_WINDOW).decode().split('\n')[1].strip()
  # 返回HWID
  HWID = hashlib.md5((cpu + HDD + MB).encode()).hexdigest()
  return HWID
```
# 调用get_hwid函数并验证
```python
Web_Hwid = requests.get('https://mrbzbz.github.io/HWID_GET_WEB/').text
#实际更改为你自己的Github Pages页面
GetLocalHwid = get_hwid()
#获取本机的HWID信息
#使用if语句进行判断
if GetLocalHwid in Web_Hwid:
  print("You are a user,Welcome.")
else:
  print("You aren't a user.")
```
