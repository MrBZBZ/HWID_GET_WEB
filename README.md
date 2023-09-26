# HWID验证网页
* 用于软件使用HWID验证用户是否合法的网页，在网页上存储内部人员的HWID以供验证
# HWID验证原理
* 程序访问已经部署好的Github Pages网页，在网页端存储好HWID信息，在本地识别到的HWID用If in语句判断是否在网页上已存储，达到验证用户的目的
# 获取本地HWID的代码（python）
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
