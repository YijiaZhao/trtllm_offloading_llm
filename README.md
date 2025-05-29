![image](https://github.com/user-attachments/assets/2603b7ba-4cfc-443b-82df-e2734bbc01fd)# deepseek offloading with trtllm

git clone https://github.com/NVIDIA/TensorRT-LLM.git

git checkout 4d711be8f4f129f8c0e3f3bdc0c8570b3f6840ce

git apply offloading.patch

python TensorRT-LLM/examples/pytorch/quickstart_advanced.py --model_dir /raid/minih/hub/models--deepseek-ai--DeepSeek-V3/snapshots/1d044fd82b15f1cedb197a288e50cc96a2c27205/ --tp_size 1

# step by step 
docker run -it  --gpus all --network=host --device=/dev/infiniband --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --shm-size="128g" -v /home/:/home --name="offloading-2nodes" nvcr.io/nvidian/sae/tensorrt_llm_release_2025_5_26 /bin/bash

trtllm-bench --model deepseek-ai/DeepSeek-V3 --model_path ../sm89/  throughput --backend pytorch --max_batch_size 2 --max_num_tokens 1500 --dataset ../bench_data/i1000o1istd0ostd0num16384.txt --tp 8 --ep 8 --streaming --warmup 0 --num_requests 32 --concurrency 32 --kv_cache_free_gpu_mem_fraction 0.05 --extra_llm_api_options ../extra_llm_yml/extra-llm-api-config-offloading.yml 

cp -r ../../tensorrt_llm/* /usr/local/lib/python3.12/dist-packages/tensorrt_llm/

# nsys
export TLLM_PROFILE_START_STOP="5-25"
nsys profile -t cuda,nvtx -c cudaProfilerApi --capture-range-end="repeat[]" --cuda-graph-trace=node -o ${nsys_save_dir}/trtllm-4node-i4096o128-${RANK}-${SLURM_PROCID} + script
