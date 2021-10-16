# README

## 编译和文件

### How to make model weight file

1. get checkpoint file

   Checkpoint path: `oss:similarity/BERT/model/SBERT_mean/checkpoint`

   > model.ckpt-32758.meta 
   >
   > model.ckpt-32758.data-00000-of-00001
   >
   > model.ckpt-32758.index

2. checkpoint file to binary weight file

   Code: `zd-algo/cuda/utils/savename.py`

   ```
   python3 zd-algo/cuda/utils/savename.py
   cd weight_file_dir
   rm *adam*
   ```

3. Weight file to half weight file

   Code: `zd-algo/cuda/sbert_mean/f2h.cpp`

   Compile: `g++ -g f2h.cpp -o f2h`

   > weight_file 是网络变量，类型是float，4字节，排列相当于 reshape(-1)
   >
   > half_weight_file，类型是half, 半浮点数，2字节，其他与weight_file 性质相同



### How to make coda file

1. Make excutable file

   Example:

   zd-algo/cuda/sbert_mean/sbert_cuda_float16/Makefile_ex

   ```
   cd zd-algo/cuda/sbert_mean/sbert_cuda_float16
   rm Makefile
   cp Makefile_ex Makefile
   make
   ./bert_v100
   ```

2. Make so file

   Example:

   zd-algo/cuda/sbert_mean/sbert_cuda_float16/Makefile_so

   ```
   cd zd-algo/cuda/sbert_mean/sbert_cuda_float16
   rm Makefile
   cp Makefile_so Makefile
   make
   # get so file bert_v100.so
   # 可以根据需要改名
   ```

3. Makefile 编译参数

   Makefile NVCC 这一行，`-arch=` 参数根据不同的gpu，指定不同的值，详情可以检索该gpu的Supported SM（streaming multiprocessor）

   ```
   #P4
   -arch=50
   #T4
   -arch=70
   #RTX2080
   -arch=75
   ```

   

