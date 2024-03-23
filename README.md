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
  chmod 0777 download-models.sh
  ./download-models.sh
  ```

### Docker image build

### 1. Build the image
  ```sh
  cd docker/
  sudodocker build . -t  deepstream:jetson
  ```
### 2. Start a container
```sh
xhost +

export DISPLAY=:0

sudo docker run -it --rm --net=host --runtime nvidia -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix -v /home/cp/Desktop/yolov8/DeepStream-Yolo:/app deepstream:jetson
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
uri=file:///app/1.mp4
....
```

Now, run the application by running the following command:

```sh
deepstream-app -c deepstream_app_config.txt
```
Debuging 
```sh
GST_DEBUG=3 deepstream-app -c deepstream_app_config.txt
```
