FROM nvidia/cuda:11.5.1-cudnn8-devel-ubuntu18.04

ENV TZ=Asia/Taipei
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt update
RUN apt install -y python3-opencv \
        libopencv-dev \
        wget \
        git \
        build-essential

RUN git clone --depth=1 https://github.com/AlexeyAB/darknet

WORKDIR darknet

RUN wget -c -N https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.weights -O /opt/yolov4.weights
RUN wget -c -N https://raw.githubusercontent.com/AlexeyAB/darknet/master/cfg/yolov4.cfg -O /opt/yolov4.cfg

RUN mkdir /opt/videos
COPY traffic.mp4 /opt/videos/traffic.mp4

EXPOSE 8070
EXPOSE 8090

RUN make -j6 GPU=0 OPENCV=1 CUDNN=0 CUDNN_HALF=0

CMD ./darknet detector demo ./cfg/coco.data ./cfg/yolov4-custom.cfg /opt/yolov4.weights /opt/videos/traffic.mp4 -json_port 8070 -mjpeg_port 8090 -ext_output  -dont_show

#EOF