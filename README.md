# Installing ROS Melodic on Jetson Nano

This guide provides step-by-step instructions for installing ROS Melodic on a Jetson Nano. It includes setting up the environment, installing necessary dependencies, and configuring the system for optimal performance.

---
## Prerequisites

Before installing ROS Melodic, ensure you have:

- A **Jetson Nano** development board (either 2GB or 4GB version)
- A **microSD card (32GB or larger)**
- A **stable internet connection** (Ethernet or Wi-Fi adapter)
- A **display, keyboard, and mouse** for setup
- A **USB Wi-Fi adapter** (if not using Ethernet)

--- 
## Step 1: Set Up Jetson Nano

1. Download and flash the **Jetson Nano Developer Kit Image** from NVIDIA:
   - [Get Started with Jetson Nano](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit)
   - Use **Balena Etcher** or a similar tool to flash the image to the microSD card.
   - Insert the microSD card into the Jetson Nano and boot it up.

2. Complete the initial setup:
   - Set up **username, password, and timezone**.
   - Enable **swap memory** for better performance:
     ```bash
     sudo fallocate -l 4G /swapfile
     sudo chmod 600 /swapfile
     sudo mkswap /swapfile
     sudo swapon /swapfile
     echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
     ```

---
## Step 2: Install Wi-Fi Adapter Driver (If Needed)

If you are using an **802.11n Wi-Fi adapter**, follow these steps to install the driver:

1. Identify the adapter:
   ```bash
   lsusb
   ```
2. Install the required packages:
   ```bash
   sudo apt update && sudo apt install dkms build-essential
   ```
3. Download and install the driver:
   ```bash
   git clone https://github.com/aircrack-ng/rtl8812au.git
   cd rtl8812au
   sudo make dkms_install
   ```
   More details can be found [here](https://askubuntu.com/questions/1349881/how-to-install-adnet-802-11n-wifi-adapter-driver).
   
---
## Step 3: Install ROS Melodic

1. Set up the sources and keys:
   ```bash
   sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
   sudo apt install curl
   curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
   ```

2. Update package index:
   ```bash
   sudo apt update
   ```

3. Install ROS Melodic Desktop version:
   ```bash
   sudo apt install ros-melodic-desktop-full
   ```

4. Initialize `rosdep`:
   ```bash
   sudo rosdep init
   rosdep update
   ```

5. Set up the ROS environment:
   ```bash
   echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```

6. Install `rosinstall` and other dependencies:
   ```bash
   sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
   ```

---
## Step 4: Verify the Installation

1. Open a new terminal and run:
   ```bash
   roscore
   ```
   If ROS is installed correctly, you should see the **ROS master** running.

2. Open another terminal and test the communication between ROS nodes:
   ```bash
   rosrun turtlesim turtlesim_node
   ```

---
## Step 5: Set Up Auto-Sourcing (Optional)

To ensure that the ROS environment is always sourced upon opening a terminal, add the following to `~/.bashrc`:
```bash
source /opt/ros/melodic/setup.bash
```

---
## Conclusion

You have successfully installed **ROS Melodic** on your Jetson Nano! You can now start developing robotic applications using ROS.

For more details, visit:
- [Official ROS Melodic Installation Guide](https://wiki.ros.org/melodic/Installation/Ubuntu)
- [NVIDIA Jetson Nano Getting Started](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit)
