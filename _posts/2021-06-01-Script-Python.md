---
layout: post
title: 批量重命名文件
author: Liang Chen
date: 2021-06-01 18:00:00 +0800
tags: [Scripts, Python]
catalog: true
---

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import os
import re

for old_name in os.listdir(path='./'):
  new_name = re.sub(r'-min', "", old_name)
  os.rename(old_name, new_name)
```
