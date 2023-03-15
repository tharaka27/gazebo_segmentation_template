# ign_gazebo_seg_template
Templete SDF for Ignition gazebo semantic segmentation dataset generation


## Installing

```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt-get update

sudo apt-get install libignition-gazebo5-dev

sudo apt-get install ros-noetic-ros-ign
```


## Testing the working condition
Test the working of ignition-gazebo

```ign gazebo -v 4 -r visualize_lidar.sdf```


## Setup communication between ros and ign (optional only needed if accessing camera data via ros)

```
sudo apt-get install ros-noetic-ros-ign-bridge
```

## Setup

1. Copy the example.sdf file to your worlds folder in the project. 
2. Create the models you need in the environment
3. Change the path to save files if needed
4. In a new terminal, run ```ign gazebo example.sdf```


## Creating model

You can use following code as an example for your the custom models
```
<model name='Car1'>
      <static>true</static>
      <link name='link'>
        <collision name='collision'>
          <pose>0 0 0 0 0 1.57079632679</pose>
          <geometry>
            <mesh>
              <scale>0.0254 0.0254 0.0254</scale>
              <uri>https://fuel.ignitionrobotics.org/1.0/openrobotics/models/hatchback blue/3/files/meshes/hatchback.obj</uri>
            </mesh>
          </geometry>
        </collision>
        <visual name='visual'>
          <pose>0 0 0 0 0 1.57079632679</pose>
          <geometry>
            <mesh>
              <scale>0.0254 0.0254 0.0254</scale>
              <uri>https://fuel.ignitionrobotics.org/1.0/openrobotics/models/hatchback blue/3/files/meshes/hatchback.obj</uri>
            </mesh>
          </geometry>
        </visual>
        <pose>0 0 0 0 -0 0</pose>
        <inertial>
          <pose>0 0 0 0 -0 0</pose>
          <mass>1</mass>
          <inertia>
            <ixx>1</ixx>
            <ixy>0</ixy>
            <ixz>0</ixz>
            <iyy>1</iyy>
            <iyz>0</iyz>
            <izz>1</izz>
          </inertia>
        </inertial>
        <enable_wind>false</enable_wind>
      </link>
      <pose>-2 -2 0 0 -0 0</pose>
      <plugin name='ignition::gazebo::systems::Label' filename='ignition-gazebo-label-system'>
        <label>40</label>
      </plugin>
      <self_collide>false</self_collide>
    </model>
```

## Saving Images

Dataset will be generated in ```/home/ros/rap/Dataset```

you can set the dataset path in 

```
<save enabled='true'>
    <path>/home/ros/rap/Dataset/</path> <!-- set path here -->
</save>
```

## Move model

To move the rotate the model along z axis for dataset generation

1. Attach velocity control plugin for the respective model

```
<plugin filename="ignition-gazebo-odometry-publisher-system" name="ignition::gazebo::systems::OdometryPublisher" />
<plugin filename="ignition-gazebo-velocity-control-system" name="ignition::gazebo::systems::VelocityControl" />
```


2. publish to the cmd_vel 

```
ign topic -t "/model/Car1/cmd_vel" -m ignition.msgs.Twist -p "linear: {x: 0.5}, angular: {z: 0.05}"
```
# gazebo_segmentation_template
