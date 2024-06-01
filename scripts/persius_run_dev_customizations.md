
    What files need to be changed?
    1) add persius_run_dev.sh
      - copy run_dev.sh to persius_run_dev.sh
      - change "-v $ISAAC_ROS_DEV_DIR:/workspaces/isaac_ros-dev \" TO "-v /ssd/workspace:/workspace/ \"
      - change reference from "install-zed-aarch64.sh" to "persius_install-zed-aarch64.sh" 
      - change reference from "dockerfile.zed" to "persius_dockerfile.zed"
    2) add persius_install-zed-aarch64.sh
      - copy files from install-zed-aarch64.sh to persius_install-zed-aarch64.sh
      - bug fix, nvidia referencing old version of the SDK (https://github.com/stereolabs/zed-ros2-wrapper/issues/226)
        - FROM -> "wget -q --no-check-certificate -O ZED_SDK_Linux.run https://stereolabs.sfo2.digitaloceanspaces.com/zedsdk/QA/JP5.1.2/ZED_SDK_Tegra_L4T35.4_v4.0.6.zstd.run"
        - TO -> "wget -q --no-check-certificate -O ZED_SDK_Linux.run https://download.stereolabs.com/zedsdk/4.1/l4t35.4/jetsons"
    3) add persius_dockerfile.zed
      - copy file from dockerfile.zed to persius_dockerfile.zed
      - bug fix, nvidia referencing old version of the SDK
        - FROM ->  "ARG ZED_SDK_MINOR=0"
        - TO -> "ARG ZED_SDK_MINOR=1" 
    4) add persius dockerfile
      - create a new dockerfile called "Dockerfile.persius" in the isaac_ros_common/docker directory
      - add the following:
        - install gazebo "sudo apt-get install ros-humble-ros-gz"
        - sudo chown admin:admin -R /workspaces 
        - build ROS packages -> "cd /workspaces && colcon build --symlink-install" 
        - echo 'export PATH="$PATH:/workspace/src/isaac_ros-dev/isaaac_ros_common/scripts"' >> ~/.bashrc
      - update the docker-compose.yml file to use the new dockerfile
        - "echo CONFIG_IMAGE_KEY=ros2_humble.user.zed.persius >> .isaac_ros_common-config"

    

