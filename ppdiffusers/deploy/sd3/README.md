# Stable Diffusion 3 高性能推理

- Paddle Inference提供Stable Diffusion 3 模型高性能推理实现，推理性能提升70%+
环境准备：
```shell
# 安装 triton并适配paddle
python -m pip install triton
python -m pip install git+https://github.com/zhoutianzi666/UseTritonInPaddle.git
python -c "import use_triton_in_paddle; use_triton_in_paddle.make_triton_compatible_with_paddle()"

# 安装develop版本的paddle，请根据自己的cuda版本选择对应的paddle版本，这里选择12.3的cuda版本
python -m pip install --pre paddlepaddle-gpu -i https://www.paddlepaddle.org.cn/packages/nightly/cu123/

# 指定 libCutlassGemmEpilogue.so 的路径
# 详情请参考 https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/kernels/fusion/cutlass/gemm_epilogue/README.md
export LD_LIBRARY_PATH=/your_dir/Paddle/paddle/phi/kernels/fusion/cutlass/gemm_epilogue/build:$LD_LIBRARY_PATH
```

高性能推理指令：
```shell
# 执行FP16推理
python  text_to_image_generation-stable_diffusion_3.py  --dtype float16 --height 512 --width 512 \
--num-inference-steps 50 --inference_optimize 1  \
--benchmark 1
```

- 在 NVIDIA A100-SXM4-40GB 上测试的性能如下：

| Paddle Inference|    PyTorch   | Paddle 动态图 |
| --------------- | ------------ | ------------ |
|       1.2 s     |     1.78 s   |    4.202 s   |
