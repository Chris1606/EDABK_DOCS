# NonGEMM Bench ISPASS 2025 AE

This repository contains all the code to implement **NonGEMM Bench**, a benchmarking flow tailored for profiling non-GEMM operators in ML models. It also contains all the scripts to generate the data used in the paper *Understanding the Performance Horizon of the Latest ML Workloads with NonGEMM Workloads* (ISPASS 2025).

---

## ⚙️ Setup Dependencies

### **Dataset Requirements**
- **Llama-2-7b-hf Weights** (Requires HuggingFace gated access). 
> Use down_model.ipynb to download.
- [COCO Images (test2017)](http://images.cocodataset.org/zips/test2017.zip)
- [COCO Annotations](http://images.cocodataset.org/annotations/image_info_test2017.zip)
- [COCO API](https://github.com/cocodataset/cocoapi)
- **ImageNet 2012 Dataset** (Training, Validation, Test)  
  Download with:

```
wget --user=<username> --password=<password> https://image-net.org/data/ILSVRC/2012/ILSVRC2012_img_val.tar
wget --user=<username> --password=<password> https://image-net.org/data/ILSVRC/2012/ILSVRC2012_img_train.tar
wget --user=<username> --password=<password> https://image-net.org/data/ILSVRC/2012/ILSVRC2012_img_test_v10102019.tar
git clone https://github.com/0429charlie/ImageNet_metadata.git
```

**Required directory structure:**

```
/media/imagenet/
    |---- train/
    |        |---- n01440764/
    |        |---- ...
    |
    |---- val/
    |        |---- n01440764/
    |        |---- ...
    |
    |---- devkit_tar/
```

---

## 🖥 Hardware  
- GPU with CUDA support  
- Verified on CUDA 12.x  

---

## 🔧 Setting Up the Environment

### **1. Create a Conda Environment**
```
cd torch_flow
conda create -n ng-torch python=3.10
```

### **2. Install Required Packages**
```
pip install -r requirements.txt
```

### **3. Install COCO API**
```
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
make install
```
> Must be placed in the same directory as `run.py`.

### **4. Configure `torch_flow/setup.sh`**
Set the following variables:
- `COCO_IMAGES`
- `COCO_ANN`
- `IMAGENET_IMAGES` (use **val** split)
- `LLAMA_WEIGHTS`

---

# 🚀 PyTorch Flow

```
cd path/to/NonGEMM-Bench/torch_flow
bash run_ispass_all.sh
```

Outputs:
- Figures 5, 6(a), 7(Top), and 8  
- Saved under `./torch_flow/summary`

---

# ⚡ ONNX Runtime (ORT) Flow

### **1. Activate environment**
```
conda activate ng-torch
```

### **2. Check CUDA Execution Provider**
```python
import onnxruntime as ort
ort.get_available_providers()
```

If CUDA EP is missing ⇒ configure `LD_LIBRARY_PATH`.

---

# 🔧 Setup LD_LIBRARY_PATH  
*Only required if PyTorch/ORT cannot find cuDNN, NCCL, or NVJitLink.*

To use CUDA-based inference, the following GPU libraries must be discoverable:

- CUDA Runtime
- cuDNN
- NCCL
- NVJitLink (CUDA 12.x JIT linker)

Since these paths may vary per Conda environment, detect them automatically:

---

## **1. Locate cuDNN**
```bash
find $CONDA_PREFIX -name "libcudnn*.so*" 2>/dev/null
```

Example result:
```
/path/to/env/lib/python3.x/site-packages/torch/lib/libcudnn.so.9
```

---

## **2. Locate NCCL**
```bash
find $CONDA_PREFIX -name "libnccl.so*" 2>/dev/null
```

---

## **3. Locate NVJitLink**
```bash
find $CONDA_PREFIX -name "libnvJitLink.so*" 2>/dev/null
```

---

## **4. Add to LD_LIBRARY_PATH**

Edit:
```
nano ~/.bashrc
```

Add:
```
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=<path_to_cudnn_folder>:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=<path_to_nccl_folder>:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=<path_to_nvjitlink_folder>:$LD_LIBRARY_PATH
```

Reload:
```
source ~/.bashrc
```

---

## 🧪 Verify ONNX Runtime CUDA EP

```python
import onnxruntime as ort
print(ort.get_available_providers())
```

Expected:
```
['CUDAExecutionProvider', 'CPUExecutionProvider']
```

---

This README ensures smooth setup of both PyTorch and ONNX Runtime flows for NonGEMM Bench.

After finishing the tutorial, run the following Readme in NonGEMM benchmark 
https://github.com/UCI-ISA-Lab/NonGEMM-Bench-ISPASS25/tree/main
