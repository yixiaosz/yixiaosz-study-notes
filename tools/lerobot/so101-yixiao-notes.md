# LeRobot SO-101

My documentation in the process of learning how to use LeRobot SO-101. 



## Setup for Teleoperation

(on MacOS 15.7.3)



### 1. Install Miniforge3

> More details on [Github Miniforge Repo](https://github.com/conda-forge/miniforge?tab=readme-ov-file#miniforge). 

```shell
cd yixiao-lerobot

# I saved the installer to my lerobot folder. 
curl -fsSLo Miniforge3.sh "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-$(uname -m).sh" 

# install Miniforge
bash Miniforge3-$(uname)-$(uname -m).sh
```

- The installation process will go through their policies etc. 
- Confirm the installation path. I installed it globally at my user folder `/Users/yixiaozhang/miniforge3`. 

- After installation, restart the terminal and it should show: `(base) yixiaozhang@YixiaoMBP lerobot %`

Once installed, the `base` environment will be activated automatically every time a terminal is opened. I turned off this auto activation by running: 

```shell
conda config --set auto_activate_base false
```



### 2. Setup Lerobot Virtual Environment

> More details on [Hugging Face](https://huggingface.co/docs/lerobot/installation). 

Create a venv with Python 3.10, using conda

```shell
conda create -y -n lerobot python=3.10
```

Install ffmpeg in the environment:

```shell
conda install ffmpeg=7.1.1 -c conda-forge
```

- All dependencies will be saved to miniforge3 folder in this case. 



### 3. Enter the lerobot venv

⚠️ Activate the lerobot conda environment, **you have to do this every time you open a shell to use lerobot!**

```shell
# check available conda virtural environments. 
# should show an *active/inactive* base venv and an *inactive* lerobot venv.
conda env list

# switch to the lerobot venv
conda activate lerobot
```

- Terminal should show: `(lerobot) yixiaozhang@YixiaoMBP lerobot %`



### 4. Clone the LeRobot library

```shell
git clone https://github.com/huggingface/lerobot.git
cd lerobot

# install the feetech SDK
pip install -e ".[feetech]"

# alternative source: tsinghua.edu mirror
pip install -e ".[feetech]" -i https://pypi.tuna.tsinghua.edu.cn/simple
```



### 5. Motor Configuration

My SO-101 was pre-configured by the vendor, so I skipped this step. 

For more details see [Hugging Face](https://huggingface.co/docs/lerobot/so101#configure-the-motors). 



### 6. Calibration

> I watched [this tutorial video on Youtube](https://youtu.be/Nhfda8h7e2I?si=e2UFdVL9cXzWocHZ) for robot calibration (it also taught motor configs). 
>

First, you need to find the correct USB serial port that connects to the follower and leader arm. 

1. **Unplug** your USB device.
2. Run `ls /dev/tty.*`
3. **Plug in** your USB device.
4. Run `ls /dev/tty.*` again.
5. The **new name** that appeared is your port.

```shell
ls /dev/tty.*
# Sample output: 
# /dev/tty.Bluetooth-Incoming-Port	/dev/tty.usbmodem5B3E1200031
# /dev/tty.debug-console
```

Alternative method recommended by Hugging Face is to use the LeRobot built-in function `lerobot-find-port`. ([Details](https://huggingface.co/docs/lerobot/so101#1-find-the-usb-ports-associated-with-each-arm))

To calibrate the follower arm, run: 

```shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5B3E1200031 \ # <- The port of your robot
    --robot.id=my_awesome_follower_arm # <- Give the robot a unique name
```

> [Video tutorial by Hugging Face](https://huggingface.co/docs/lerobot/so101#calibration-video)

Note: the system will ask you to `Move my_awesome_follower-arm SO101Follower to the middle of its range of motion`. The **middle of its range of motion** means this:

![midd-of-its-range-of-motion](./assets/midd-of-its-range-of-motion.jpg)



To calibrate the leader arm, run: 

```shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5B3E1223411 \ # <- The port of your robot
    --teleop.id=my_awesome_leader_arm # <- Give the robot a unique name
```



### 7. Begin Teleoperation

> More details on [Hugging Face](https://huggingface.co/docs/lerobot/il_robots). 
>

```shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5B3E1200031 \
    --robot.id=my_awesome_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5B3E1223411 \
    --teleop.id=my_awesome_leader_arm
```

- My experience was that the system may ask me to recalibrate the follower arm because of error code `95 Mismatch between calibration values in the motor and the calibration file or no calibration file found`. After this extra follower arm calibration, the teleoperation should begin. 

The teleoperate command will automatically:

1. Identify any missing calibrations and initiate the calibration procedure.
2. Connect the robot and teleop device and start teleoperation.



