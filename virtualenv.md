## 虚拟环境



### site-packages

```shell
pip freeze > requirements.txt
pip install -r requirements.txt
```



### Python

+ 安装

  ```shell
  pip install virtualenv
  ```

+ 创建

  ```shell
  virtualenv [-p /usr/bin/python3] venv(虚拟环境名称/路径)
  ```

+ 激活

  ````shell
  source ./venv/bin/activate
  ````

+ 退出

  ```shell
  deactivate
  ```




### Miniconda

+ 查看

  ```shell
  conda info --envs
  ```

+ 创建

  ```shell
  conda create --prefix=/opt/miniconda3/envs/venv python=3.7.3 -y
  ```

+ 激活

  ```shell
  conda activate venv
  ```

+ 退出

  ``` shell
  conda deactivate
  ```






