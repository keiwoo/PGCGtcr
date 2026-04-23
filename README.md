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
There is no feature or function needing specific version of package, you can run  in your existing Python environment conveniently. 

### 2. Download the model
The fine-tuned models are at Hugging Face, you should download and put them in right place. Please follow the instructions in every model file folder.

### 3. Run the model
```
from transformers import T5ForConditionalGeneration, T5Tokenizer    
import torch

torch.cuda.manual_seed_all(42)
device = torch.device('cuda:1')

model_path = 'model/VDJdb'
model = T5ForConditionalGeneration.from_pretrained(
    model_path, 
    dtype=torch.float16,    # If your device does not support `BF16` precision mode
                            # (GPU based on NVIDIA's Ampere and its subsequent architectures), you should comment this line.
    local_files_only=True
    ).to(device)
model.eval()
tokenizer = T5Tokenizer.from_pretrained(model_path, local_files_only=True)

sample_kwargs = {
    'do_sample': True,
    'top_k': 9,
    'top_p': 0.93,
    'temperature': 0.85,
    'num_return_sequences': 16,
}
beam_kwargs = {
    'num_beams': 16,
    'num_return_sequences': 16,
}
inputs = "CAI:AAGIGILTV"
tokenized = tokenizer('# '+" ".join(inputs), return_tensors="pt", ).to(device)
# outputs = model.generate(**tokenized, **sample_kwargs) # If you want different sequences every generation, uncomment this line. 
outputs = model.generate(**tokenized, **beam_kwargs) # If you want same sequences every generation (most possible sequence the model think), uncomment this line.

tcr = tokenizer.batch_decode(outputs, skip_special_tokens=True)
tcr = [i.replace(' ', '') for i in tcr]
print(tcr)
# ['CAISESNFGNEKLTF', 'CAISESGLGQPQHF', 'CAISESGGGADGLTF', ...]
```


## Web server
We also provide a web server for users to test their TCR-peptide pairs without running code. The web server is at [PGCGtcr Web Server](https://fca_icdb.mpu.edu.mo/pgcgtcr).
