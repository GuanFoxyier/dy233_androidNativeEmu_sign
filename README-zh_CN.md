# dy233_androidNativeEmu_sign
使用 ExAndroidNativeEmu 模拟某音 23.3.0 签名

## 怎么跑起来
1. git clone https://github.com/maiyao1988/ExAndroidNativeEmu
2. 从[豌豆荚](https://www.wandoujia.com/apps/7461948/history_v230301)下载 dy233.apk
3. apktool d dy233.apk, 拿到 libmetasec_ml.so
4. 把 "libmetasec_ml.so" 放到相对目录 "vfs/data/data/com.ss.android.ugc.aweme/" 下
5. 把 "example_dy233.py" 放到项目主目录下
6. 修改在 "androidemu/java/jni_env.py" 文件中的 `get_object_ref_type` 函数

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
7. 修改 "scheduler.py" 中的代码

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