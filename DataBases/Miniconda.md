## 虚拟环境

+ 查看

  ```
  conda info --envs
  ```

+ 创建

  ```
  conda create --prefix=/opt/miniconda3/envs/sxznyjpt python=3.7.3 -y
  ```

+ 激活

  ```
  conda activate sxznyjpt
  ```

+ 退出

  ``` 
  conda deactivate
  ```

  

## site-packages

``` 
pip freeze >requirements.txt
pip install -r requirements.txt
```



