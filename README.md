# dy233_androidNativeEmu_sign
use ExAndroidNativeEmu to emulate dy 23.3.0 for X-Medusa and X-Helios

## how to run
1. git clone https://github.com/maiyao1988/ExAndroidNativeEmu
2. download dy233.apk from [wandoujia](https://www.wandoujia.com/apps/7461948/history_v230301)
3. apktool d dy233.apk, get the libmetasec_ml.so from apk
4. place "libmetasec_ml.so" in the relative directory "vfs/data/data/com.ss.android.ugc.aweme/"
5. place "example_dy233.py" in the directory of project
6. modify the function `get_object_ref_type` in "androidemu/java/jni_env.py"

before:
```python
def get_object_ref_type(self, mu, env):
    raise NotImplementedError()
```
after:
```python
def get_object_ref_type(self, mu, env):
    return 0
```
7. modify the code in "scheduler.py"

before:
```python
if self.__pid not in self.__tasks_map:
    #主线程退出，退出调度循环
    logging.debug("main_thead tid [%d] exit exec return"%self.__pid)
    if (clear_task_when_return):
        #clear all unfinished task
        self.__tasks_map.clear()
        return
```
after:
```python
if self.__pid not in self.__tasks_map:
    #主线程退出，退出调度循环
    logging.debug("main_thead tid [%d] exit exec return"%self.__pid)
    if (clear_task_when_return):
        #clear all unfinished task
        self.__tasks_map.clear()
        self.__ordered_tasks_list.clear()
        return
```

8. python3 example_dy233.py