## Kaggle Docker


* To build the container

        ./buildKaggleDocker.sh

* To run the container

        nvidia-docker run --rm -it -v /home/mra/misc:/home/mra/misc --net=host -e DISPLAY -v          $HOME/.Xauthority:/root/.Xauthority kaggle_docker


# /home/mra/misc is path to local working dir
# kaggle_docker is the container's name



