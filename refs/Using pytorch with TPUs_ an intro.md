# **Using pytorch with TPUs: an intro**

**Hello there, it’s Tensor.**

This is just a scrappy guide on how I got to set up **pytorch training** with **Google TPUs** after many trials \[especially the ones on **Kaggle/Colab**, **TPUv3.8**\]. As a GPU-poor researcher, I used up my weekly 30 GPU hours on Kaggle, so I decided to find a way to use the weekly 20 TPU hours too \[and also because I am a fan of **Google**\]. Before I continue, here is the official repo: [**pytorch/xla**](https://github.com/pytorch/xla/blob/master/API_GUIDE.md)**.**

First, about TPUs (**Tensor Processing Units**), they are Google’s AI-specific accelerator chips, using application specific integrated circuits (**ASICs**), they use the **XLA** compiler rather than **CUDA**, but still work with Tensorflow, Pytorch and JAX. They are more power-efficient than GPUs and specialized for AI training workloads, but let’s not digress to the architecture **:)**

**Setup**  
For a basic training setup, without distributed parallel processing across several TPU chips (a single TPU has **8** or more **‘TensorCores’,** depending on the version), I will show an example for just training a simple model with TPUs.

First, you make the necessary imports: 

| pip install torch-xla *\# install for local development* |
| :---- |

| import torch\_xla as xlaimport torch\_xla.core.xla\_model as xmprint(xla.devices()) \#to check the TPU cores and count |
| :---- |

Next you just have to create a **device** variable, which the **model** and **data** will be mapped to:

| device \= xla.device() *\#or xm.xla\_device()*print(device) |
| :---- |

The rest of the code would use normal pytorch functionalities. Next major area of change is the t**raining loop/function**:

| model \= Classifier().to(device) \# move model to tpu loss\_fn \= nn.CrossEntropyLoss()optimizer \= optim.Adam(model.parameters(), lr=lr) def \_trainer():    for epoch in range(epochs):        for \_, (image, label) in enumerate(train\_loader):            with xla.step(): \# add this line                \# move image and labels to device                image, label \= image.to(device), label.to(device)                output \= model(image)                loss \= loss\_fn(output, label)                loss.backward()                losses.append(loss.item())                \# gradient update                xm.optimizer\_step(optimizer) \# replaces optimizer.step()                xm.mark\_step() \# for gradient tracing                 optimizer.zero\_grad()\_trainer() |
| :---- |
|  |

In summary, these are the main changes:

- Import torch\_xla, and the core xla\_model class, and create a device variable for **tensor mapping/transfer.**  
- Wrap the training loop/gradient computation code with xla.step()  
- Replace optimizer.step() with xm.optimizer\_step(optimizer)  
- Add xm.mark\_step() \[xm is the import symbol for torch\_xla.core.xla\_model\]  
- Remove all cuda-related code, as TPUs use the **XLA** compiler.

# 

# **Comparison**

Compared to GPUs, Kaggle TPU VMs are accessed in queues, which might cause some latency. As for this comparison, I am using the **Tesla P100** and Google **TPU v3.8** (current version is **v6**), both on Kaggle. I ran the same training run for a tiny diffusion model, on 1000 images. On the GPU, it took **4.46** minutes, while on the TPU, it took **3.37** minutes. Even with this, TPUs usually start off slow while computing the graph, then they get faster.

**Conclusion**  
For a comprehensive example, check out my **[github gist](https://gist.github.com/kelechi-c/3fec230de4cd5f37039dfcddfeeb048b)** on training a **tiny** **diffusion model** using a TPU.  
I will update the article later on using **multi-processing** for all TPU cores, and also with other necessary additions, which creates a significant speedup. Feedbacks and corrections are highly appreciated and needed **:)** 

**Thanks**\!  
   
**Other useful guides you should check out:**

* [https://github.com/pytorch/xla/blob/master/API\_GUIDE.md](https://github.com/pytorch/xla/blob/master/API_GUIDE.md)  
* [https://github.com/pytorch/xla](https://github.com/pytorch/xla)  
* [https://pytorch.org/xla/](https://pytorch.org/xla/)