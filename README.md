# DeepStream-Yolo Jetson Nano

Install git
```sh
sudo apt install git
```
Clone git repository
```sh
git clone https://github.com/bijoycp/DeepStream-YOLOv8-Jetson-Nano.git
```

### install jtop
  ```sh
  pip install jetson-stats
  jtop
  ```

### download models
  ```sh
  cd DeepStream-YOLOv8-Jetson-Nano
  chmod 0777 download_models.sh
  ./download_models.sh
  ```

### Docker image build

### 1. Build the image
  ```sh
  cd docker/
  sudo docker build . -t  deepstream:jetson
  cd ..
  ```
### 2. Start a container
```sh
xhost +

export DISPLAY=:0

sudo docker run -it --rm --net=host --runtime nvidia -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix -v $PWD:/app deepstream:jetson
```
## Running the Application

### 1. Build the application

```sh
CUDA_VER=10.2 make -C nvdsinfer_custom_impl_Yolo
```

### 2. Start the application

Edit the `config_infer_primary_yoloV8_onnx.txt` file according to your model 

```
[property]
...
onnx-file=model/yolov8n.onnx
...
```

convert model into fp16
```
/usr/src/tensorrt/bin/trtexec --onnx=model/yolov8n.onnx --fp16 --saveEngine=model/yolov8n_fp16.engine
```

`config_infer_primary_yoloV8_onnx.txt`
```
[property]
...
model-engine-file=model/yolov8n_fp16.engine
...
```
Add video source  `deepstream_app_config.txt`  

```sh
[source0]
....
uri=file:///app/video/3.mp4
....
```

Now, run the application by running the following command:

```sh
deepstream-app

 -c deepstream_app_config.txt
```
Debuging 
```sh
GST_DEBUG=3 deepstream-app -c deepstream_app_config.txt
```
### Usefull links
[NVIDIA DeepStream SDK on Jetson](https://developer.nvidia.com/embedded/deepstream-on-jetson-downloads-archived)

[YOLOv8-DeepStream-TRT-Jetson](https://wiki.seeedstudio.com/YOLOv8-DeepStream-TRT-Jetson/)
