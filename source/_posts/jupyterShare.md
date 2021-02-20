---
title: 共享Jupyter notebook环境
date: 2019-10-02 00:00:00
tags:
	- jupyter notebook
	- python
---
1. 生成配置文件
	```
	jupyter notebook --generate-config
	```

2. 生成密码

   ```
   In [1]: from notebook.auth import passwd
   In [2]: passwd()
   Enter password: 
   Verify password: 
   Out[2]: 'sha1:***************************************************'
   ```

3. 修改config文件

   ```
   /[user]/.jupyter/jupyter_notebook_config.py 
   ```

   ```
   c.NotebookApp.ip='*'	# allow all network
   c.NotebookApp.password = u'sha:******************'
   c.NotebookApp.open_browser = False
   c.NotebookApp.port =8888 #port to share
   ```

4. 启动

   ```
jupyter notebook
   ```

   
   
   
   
   
   
   

