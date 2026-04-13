This repository contains relevant information of our research article *'Prefix-guided conditional generation of TCR by fine-tunning large protein language model'*.

## Reproduce quickly

### 1. Setup running environment
Though the main Python packages we use are
```
python=3.13.9
transformers=4.57.1
sentencepiece=0.2.1
torch=2.9.1
numpy=2.2.6
pandas=3.0.1
cuda=13.0
```
There is no feature or function needing specific version of package, you can run `RoBERTcr.py` in your existing Python environment conveniently. 

> If your device does not support `BF16` precision mode (GPU based on NVIDIA's Ampere and its subsequent architectures), you should set `bf16=False` which will cost 4x time more than at most.

### 3. Run the model



## Web server
We also provide a web server for users to test their TCR-peptide pairs without running code. The web server is at [ Web Server](https://fca_icdb.mpu.edu.mo/).
