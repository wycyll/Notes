
|特性|CUDA Core|Tensor Core|
|---|---|---|
|类型|标量 ALU|矩阵乘法单元|
|计算方式|标量/向量|小矩阵|
|精度|FP32/FP64|FP16/TF32/INT8 等|
|使用方式|普通 CUDA kernel|WMMA / cuBLAS|
|速度|普通|非常快|
