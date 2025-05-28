# deepseek offloading with trtllm

git clone https://github.com/NVIDIA/TensorRT-LLM.git

git checkout 4d711be8f4f129f8c0e3f3bdc0c8570b3f6840ce

git apply offloading.patch

python TensorRT-LLM/examples/pytorch/quickstart_advanced.py --model_dir /raid/minih/hub/models--deepseek-ai--DeepSeek-V3/snapshots/1d044fd82b15f1cedb197a288e50cc96a2c27205/ --tp_size 1

# step by step 
docker run -it -P --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --device=/dev/infiniband --shm-size="128g" -v /home/:/home --name="offloading" nvcr.io/nvidian/sae/tensorrt_llm_release_2025_5_26 /bin/bash

cp -r ../../tensorrt_llm/* /usr/local/lib/python3.12/dist-packages/tensorrt_llm/![Uploading image.png…]()
