# Soft Actor-Critic
Soft actor-critic is a deep reinforcement learning framework for training maximum entropy policies in continous domains. [TODO.before_release: paper reference]

# Getting Started

Soft Actor-Critic can be run either locally or through Docker.

## Prerequisites

You will need to have [Docker](https://docs.docker.com/engine/installation/) and [Docker Compose](https://docs.docker.com/compose/install/) installed unless you want to run the environment locally.

Most of the models require a [Mujoco](https://www.roboti.us/license.html) license.

## Docker installation

If you want to run the Mujoco environments, the docker environment needs to know where to find your Mujoco license key (`mjkey.txt`). You can either copy your key into `<PATH_TO_THIS_REPOSITY>/.mujoco/mjkey.txt`, or you can specify the path to the key in your environment variables:

```
export MUJOCO_LICENSE_PATH=<path_to_mujoco>/mjkey.txt
```

Once that's done, you can run the Docker container with

```
docker-compose up
```

Docker compose creates a Docker container named `soft-actor-critic` and automatically sets the needed environment variables and volumes.

You can access the container with the typical Docker [exec](https://docs.docker.com/engine/reference/commandline/exec/)-command, i.e.

```
docker exec -it soft-actor-critic bash
```

See examples section for examples of how to train and simulate the agents.

To clean up the setup:
```
docker-compose down
```

## Local installation

To get the environment installed correctly, you will first need to clone rllab [rllab](https://github.com/rll/rllab), and have its path added to your PYTHONPATH environment variable.

1. Clone rllab
```
cd <installation_path_of_your_choice>
git clone https://github.com/rll/rllab.git
cd rllab
git checkout b3a28992eca103cab3cb58363dd7a4bb07f250a0
export PYTHONPATH=$(pwd):${PYTHONPATH}
```

2. [Download](https://www.roboti.us/index.html) and copy mujoco files to rllab path:
If you're running on OSX, download https://www.roboti.us/download/mjpro131_osx.zip instead, and copy the `.dylib` files instead of `.so` files.
```
mkdir -p /tmp/mujoco_tmp && cd /tmp/mujoco_tmp
wget -P . https://www.roboti.us/download/mjpro131_linux.zip
unzip mjpro131_linux.zip
mkdir <installation_path_of_your_choice>/rllab/vendor/mujoco
cp ./mjpro131/bin/libmujoco131.so <installation_path_of_your_choice>/rllab/vendor/mujoco
cp ./mjpro131/bin/libglfw.so.3 <installation_path_of_your_choice>/rllab/vendor/mujoco
cd ..
rm -rf /tmp/mujoco_tmp
```

3. Copy your Mujoco license key (mjkey.txt) to rllab path:
```
cp <mujoco_key_folder>/mjkey.txt <installation_path_of_your_choice>/rllab/vendor/mujoco
```

TODO.before_release: change to public url and change `softqlearning-private` folder name to match
4. Clone softqlearning
```
cd <installation_path_of_your_choice>
git clone https://github.com/haarnoja/softqlearning-private.git
cd softqlearning-private
```

5. Create and activate conda environment
```
cd softqlearning-private # TODO.before_release: update folder name
conda env create -f environment.yml
source activate sac
```

The environment should be ready to run. See examples section for examples of how to train and simulate the agents.

Finally, to deactivate and remove the conda environment:
```
source deactivate
conda remove --name sac --all
```

## Examples
### Training and simulating an agent
1. To train the agent
```
python ./examples/mujoco_all_sac.py --env=swimmer --log_dir="/root/softqlearning-private/data/swimmer-experiment"
```

2. To simulate the agent (*NOTE*: This step currently fails with the Docker installation, due to missing display.)
```
python ./scripts/sim_policy.py /root/softqlearning-private/data/swimmer-experiment/itr_<iteration>.pkl
```

`mujoco_all_sac.py` contains several different environments and there are more example scripts available in the  `/examples` folder. For more information about the agents and configurations, run the scripts with `--help` flag. For example:
```
python ./examples/mujoco_all_sac.py --help
usage: mujoco_all_sac.py [-h]
                         [--env {ant,walker,swimmer,half-cheetah,humanoid,hopper}]
                         [--exp_name EXP_NAME] [--mode MODE]
                         [--log_dir LOG_DIR]
```

# Credits
The soft actor-critic algorithm was developed by Tuomas Haarnoja under the supervision of [Prof. Sergey Levine](https://people.eecs.berkeley.edu/~svlevine/) and [Prof. Pieter Abbeel](https://people.eecs.berkeley.edu/~pabbeel/) at UC Berkeley. Special thanks to [Vitchyr Pong](https://github.com/vitchyr), who wrote some parts of the code, and [Kristian Hartikainen](https://github.com/hartikainen) who helped testing and documenting the code. The work was supported by [Berkeley Deep Drive](https://deepdrive.berkeley.edu/).

# Reference
TODO.before_release