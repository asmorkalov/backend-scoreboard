# use default OpenCV CI environment to get all required 3rdparties
FROM quay.io/opencv-ci/opencv-ubuntu-20.04:20230413

USER root

# Disable interactive installation mode
ENV DEBIAN_FRONTEND=noninteractive

RUN pip3 install --upgrade pip setuptools wheel

# Copy local directories
COPY ./test /root/test
COPY ./setup /root/setup

# Install test report dependencies
RUN pip3 install --no-cache-dir -r /root/setup/requirements_report.txt

RUN pip3 install --no-cache-dir opencv-python-headless==4.8.0.74

COPY ./opencv-onnx-conformance-proxy /root/opencv-onnx-conformance-proxy
RUN cd /root/opencv-onnx-conformance-proxy && pip3 install . && cd -

ENV ONNX_BACKEND="opencv_onnx_proxy"

# Install dependencies
RUN pip install onnx

CMD . /root/setup/docker-setup.sh && \
    pytest /root/test/test_backend.py --onnx_backend=${ONNX_BACKEND} -k 'not _cuda' -v
