# Docker image for A1111 Stable Diffusion Web UI, Kohya_ss and ComfyUI

ROCm fork. In progress. 

## Installs

* Ubuntu 22.04 LTS
* ROCm 6.1
* Python 3.10.12
* [Automatic1111 Stable Diffusion Web UI](
  https://github.com/AUTOMATIC1111/stable-diffusion-webui) 1.7.0
* [Dreambooth extension](
  https://github.com/d8ahazard/sd_dreambooth_extension) 1.0.14
* [Deforum extension](
  https://github.com/deforum-art/sd-webui-deforum)
* [ControlNet extension](
  https://github.com/Mikubill/sd-webui-controlnet) v1.1.424
* [After Detailer extension](
  https://github.com/Bing-su/adetailer) v23.11.1
* [Locon extension](
  https://github.com/ashleykleynhans/a1111-sd-webui-locon)
* [ReActor extension](https://github.com/Gourieff/sd-webui-reactor) (replaces roop)
* [Inpaint Anything extension](https://github.com/Uminosachi/sd-webui-inpaint-anything)
* [Infinite Image Browsing extension](https://github.com/zanllp/sd-webui-infinite-image-browsing)
* [CivitAI extension](https://github.com/civitai/sd_civitai_extension)
* [CivitAI Browser+ extension](https://github.com/BlafKing/sd-civitai-browser-plus)
* [Kohya_ss](https://github.com/bmaltais/kohya_ss) v22.4.0
* [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
* [ComfyUI Manager](https://github.com/ltdrdata/ComfyUI-Manager)
* Torch 2.4.0
* [xformers 0.0.23](https://github.com/ROCmSoftwarePlatform/xformers/)
* [sd_xl_base_1.0.safetensors](
  https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors)
* [sd_xl_refiner_1.0.safetensors](
  https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0.safetensors)
* [sdxl_vae.safetensors](
  https://huggingface.co/madebyollin/sdxl-vae-fp16-fix/resolve/main/sdxl_vae.safetensors)
* [inswapper_128.onnx](
  https://github.com/facefusion/facefusion-assets/releases/download/models/inswapper_128.onnx)
* [runpodctl](https://github.com/runpod/runpodctl)
* [croc](https://github.com/schollz/croc)
* [rclone](https://rclone.org/)
* [Application Manager](https://github.com/ashleykleynhans/app-manager)

## Building the Docker image

Since the Stable Diffusion models are pretty large, you will need at least
8GB of system memory (not GPU VRAM) to build this image.

The image **CAN** be built on a `t3a.large` AWS EC2 instance
which has 2 x vCPU and 8GB of system memory.  It **CANNOT** be built on
any instances with less memory, eg. `t3a.medium`.

```bash
# Clone the repo
git clone --recursive https://github.com/samexner/stable-diffusion-ROCker.git

# Download the models
cd stable-diffusion-ROCker
wget https://huggingface.co/runwayml/stable-diffusion-v1-5/resolve/main/v1-5-pruned.safetensors
wget https://huggingface.co/stabilityai/sd-vae-ft-mse-original/resolve/main/vae-ft-mse-840000-ema-pruned.safetensors
wget https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors
wget https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0.safetensors
wget https://huggingface.co/madebyollin/sdxl-vae-fp16-fix/resolve/main/sdxl_vae.safetensors

# Build and tag the image
docker build -t username/image-name:1.0.0 .

# Log in to Docker Hub
docker login

# Push the image to Docker Hub
docker push username/image-name:1.0.0
```

## Running Locally

### Install AMD ROCm Driver

- [Linux](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/tutorial/quick-start.html)

### Start the Docker container

```bash
docker run -it -v /workspace -it --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --device=/dev/kfd --device=/dev/dri --group-add video --ipc=host --shm-size 8G -p 3000:3001 -p 3010:3011 -p 3020:3021 -p 6006:6066 -p 8888:8888 -p 8000:8000 -e JUPYTER_PASSWORD=Jup1t3R! -e ENABLE_TENSORBOARD=1 psnrainbowtrout/sdrocker:1.0.0
```

You should substitute the image name and tag with your own.

## Acknowledgements

1. [RunPod](https://runpod.io?ref=2xxro4sy) for providing most
   of the [container](https://github.com/runpod/containers) code.
2. [Bernard Maltais](https://github.com/bmaltais) (core developer of Kohya_ss)
   for assisting with optimizing the Docker image.
3. [Ashley Kleynhans](https://github.com/ashleykleynhans) for the original image configured for NVidia.

## Community and Contributing

Pull requests and issues on [GitHub](https://github.com/ashleykleynhans/stable-diffusion-docker)
are welcome. Bug fixes and new features are encouraged.

