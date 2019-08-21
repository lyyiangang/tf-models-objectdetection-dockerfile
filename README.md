## dockerfile for tensorflow models repo.
this dockerfile could help you build your own image for tensorflow models repo.
## how to use
1. build image
    ```
    docker build -f ./tf-model.dockerfile -t tf-models .
    ```
2. run a container
    ```
    export TF_MODELS_REPO=''
    xhost +local:${USER}
    docker run  --gpus all -it --rm -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix \
    -u $(id -u):$(id -g) \
    -v ${TF_MODELS_REPO}:/project/models \
    -w /project/models \
    --name tf-models-env \
    tf-models
    ```
3. now you should be in the container

    - set enviroment variables:
        ```
        # From tensorflow/models/research/
        export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
        ```

    - generate proto files
        ```
        # From tensorflow/models/research/
        protoc object_detection/protos/*.proto --python_out=.
        ```

    - test
        ```
        python3 object_detection/builders/model_builder_test.py
        ```