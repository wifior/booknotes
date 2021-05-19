一.环境安装

​    1.anaconda的下载安装:https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

​    2.在shell中运行

```xml
bash Anaconda3-5.3.1-Linux-x86_64.sh
Please, press ENTER to continue
>>>
------------------------------------------------------
Do you accept the license terms? [yes|no]
[no] >>> yes
-------------------------------------------------------
Anaconda3 will now be installed into this location:
/home/xxx/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below
--------------------------------------------------------
Do you wish the installer to initialize Anaconda3
in your /home/xxx/.bashrc ? [yes|no]
[no] >>> yes
---------------------------------------------------------
Do you wish to proceed with the installation of Microsoft VSCode? [yes|no]
>>> yes

```

 bash Anaconda3-5.3.1-Linux-x86_64.sh ----->enter----->yes---->enter(确认安装目录)--->yes(安装并初始化)----->yes(安装vscode)

​    3. 编辑/etc/profile

```xml
sudo vi /etc/profile
export PATH=/home/xxx/anaconda3/bin:$PATH
source /etc/profile
```

​     4.在shell中输入python 

```xml
$:python3
Python 3.7.0 (default, Jun 28 2018, 13:15:42) 
[GCC 7.2.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

​    5.查看anaconda版本

```
$: conda --version
conda 4.5.11

```

​         看到Anaconda,Inc.on linux就说明安装成功了,我们第一步就做完了.

二.TensorFlow安装

1.在shell 中使用 

```xml
conda create -n tensorflow python=3.7
Solving environment: done
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use:
# > source activate tensorflow
#
# To deactivate an active environment, use:
# > source deactivate
#
```

2.激活conda环境

```xml
source activate tensorflow
(tensorflow) xxx@xxx:~/anaconda3/envs$ 
```

3.安装tensorflow

```xml
pip install --ignore-installed -upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.13.1-cp37-cp37m-linux_x86_64.whl
```

4.在命令行里输入python,并输入以下代码:

```python
 import tensorflow as tf
 message = tf.constant('Welcome to the exciting world of Deep Neural Networks!')
 with tf.Session() as sess:
       print(sess.run(message).decode())
... 
2019-09-03 17:12:08.387886: I tensorflow/core/platform/cpu_feature_guard.cc:141] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
2019-09-03 17:12:08.775026: I tensorflow/core/platform/profile_utils/cpu_utils.cc:94] CPU Frequency: 3408000000 Hz
2019-09-03 17:12:08.783710: I tensorflow/compiler/xla/service/service.cc:150] XLA service 0x560fbb8e1020 executing computations on platform Host. Devices:
2019-09-03 17:12:08.783776: I tensorflow/compiler/xla/service/service.cc:158]   StreamExecutor device (0): <undefined>, <undefined>
Welcome to the exciting world of Deep Neural Networks!
>>> 
```

5.退出conda环境

```xml
source deactivate
```

>   TensorFlow安装过程解读分析
>
>  Google 使用 wheel 标准分发 TensorFlow，它是 .whl 后缀的 ZIP 格式文件。Python 3.6 是  Anaconda 3 默认的 Python 版本，且没有已安装的 wheel。在编写本教程时，Python 3.6 支持的 wheel 仅针对  Linux/Ubuntu，因此，在创建 TensorFlow 环境时，这里指定 Python 3.5。接着新建 conda 环境，命名为  tensorflow，并安装 pip，python，wheel 及其他软件包。
>
>  conda 环境创建后，调用 source activate/activate 命令激活环境。在激活的环境中，使用 pip install  命令安装所需的 TensorFlow（从相应的 TensorFlow-API URL下载）。尽管有利用 conda forge 安装  TensorFlow CPU 的 Anaconda 命令，但 TensorFlow 推荐使用 pip install。在 conda 环境中安装  TensorFlow 后，就可以禁用了。现在可以执行第一个 TensorFlow 程序了。



























