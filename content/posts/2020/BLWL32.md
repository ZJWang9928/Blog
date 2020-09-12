---
title: "BLWL[32] Pyinstaller 打包时报错 TypeError: an integer is required (got type bytes)"
date: 2020-08-02T16:50:51+08:00
categories: ["Better Life With Linux"]
tags: ["python", "Linux", "pyinstall", "pip"]
draft: false
---

## 报错示例

```
Traceback (most recent call last):
  File "/home/godlovesjonny/.local/bin/pyinstaller", line 8, in <module>
    sys.exit(run())
  File "/usr/lib/python3.8/site-packages/PyInstaller/__main__.py", line 111, in run
    run_build(pyi_config, spec_file, **vars(args))
  File "/usr/lib/python3.8/site-packages/PyInstaller/__main__.py", line 63, in run_build
    PyInstaller.building.build_main.main(pyi_config, spec_file, **kwargs)
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 844, in main
    build(specfile, kw.get('distpath'), kw.get('workpath'), kw.get('clean_build'))
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 791, in build
    exec(code, spec_namespace)
  File "/home/godlovesjonny/Jonny/Installs/v2rayL-master/v2rayL-GUI/v2rayLui.spec", line 18, in <module>
    pyz = PYZ(a.pure, a.zipped_data,
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/api.py", line 98, in __init__
    self.__postinit__()
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/datastruct.py", line 158, in __postinit__
    self.assemble()
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/api.py", line 128, in assemble
    self.code_dict = {
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/api.py", line 129, in <dictcomp>
    key: strip_paths_in_code(code)
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/utils.py", line 652, in strip_paths_in_code
    consts = tuple(
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/utils.py", line 653, in <genexpr>
    strip_paths_in_code(const_co, new_filename)
  File "/usr/lib/python3.8/site-packages/PyInstaller/building/utils.py", line 660, in strip_paths_in_code
    return code_func(co.co_argcount, co.co_kwonlyargcount, co.co_nlocals, co.co_stacksize,
TypeError: an integer is required (got type bytes)
```

## 解决方案

报错的原因在于所使用的 Pyinstaller 是用 `pip install pyinstall` 安装的。  

改成

```
pip install https://github.com/pyinstaller/pyinstaller/archive/develop.tar.gz
```

重新安装一次即可。  
