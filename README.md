# deepseek offloading with trtllm

git clone https://github.com/NVIDIA/TensorRT-LLM.git

git checkout 5b1c88de8dd997761feefb25382cfc7f951a34b2 

git apply offloading.patch

python TensorRT-LLM/examples/pytorch/quickstart_advanced.py --model_dir /raid/minih/hub/models--deepseek-ai--DeepSeek-V3/snapshots/1d044fd82b15f1cedb197a288e50cc96a2c27205/ --tp_size 1
