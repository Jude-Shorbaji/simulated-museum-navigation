# ISCAS Museum

A detailed indoor Gazebo model of a multi-room museum hall — open galleries,
connecting corridors, and walls with textured surfaces. It's a ready-made
environment for testing navigation, mapping (SLAM), and obstacle avoidance
without building a world from scratch.

This is a **Gazebo model** (a single static structure), not a full world. You
spawn it *into* a world — add your own ground plane, lighting, and physics, then
drop the museum in as the scenery.

## What's inside

| File | Purpose |
|------|---------|
| `model.sdf` | The model definition — a single static link with the museum mesh. |
| `model.config` | Fuel metadata (name, author, SDF version). |
| `meshes/ISCAS_museum.dae` | The COLLADA mesh — the museum geometry. |
| `materials/textures/` | Wall and floor textures referenced by the mesh. |

## Use it in your world

The fastest way — reference it by URI in any world SDF. Gazebo fetches it on
launch, the same way it pulls models from Gazebo Fuel:

```xml
<include>
  <uri>https://assets.leetrobotics.com/1.0/leetrobotics/models/iscas_museum</uri>
  <pose>0 0 0 0 0 0</pose>
</include>
```

Drop that inside your `<world>`, alongside a ground plane and a sun:

```xml
<world name="museum_world">
  <include>
    <uri>https://fuel.gazebosim.org/1.0/OpenRobotics/models/Sun</uri>
  </include>
  <include>
    <uri>https://fuel.gazebosim.org/1.0/OpenRobotics/models/Ground Plane</uri>
  </include>
  <include>
    <uri>https://assets.leetrobotics.com/1.0/leetrobotics/models/iscas_museum</uri>
    <pose>0 0 0 0 0 0</pose>
  </include>
</world>
```

Then launch it:

```bash
gz sim museum_world.sdf
```

## Use it offline

Prefer a local copy? Download and extract the bundle, then point Gazebo at it:

```bash
# Make a folder for the model in your ros workspace and cd into it
mkdir -p ~/ros2_ws/models/iscas_museum && cd ~/ros2_ws/models/iscas_museum

# Download and extract the bundle
curl -sSLO https://assets.leetrobotics.com/1.0/leetrobotics/models/iscas_museum/tip/iscas_museum.zip
unzip -o iscas_museum.zip && rm iscas_museum.zip

# Tell Gazebo where to find it (add to .bashrc to persist)
export GZ_SIM_RESOURCE_PATH=$HOME/ros2_ws/models:$GZ_SIM_RESOURCE_PATH
```

With the path set, you can reference it locally as `model://iscas_museum` inside
any world.

## Spawn a robot inside it

Once the world is running, spawn a robot into the museum and start driving:

```bash
ros2 run ros_gz_sim create -world museum_world -name turtlebot3 \
  -x 0 -y 8 -z 0.1 -file turtlebot3_burger/turtlebot3_burger.sdf
```

## Good to know

- **It's static.** The museum has collision geometry but no joints or moving
  parts — it's scenery, so your robot's LiDAR and bumpers see the walls but
  nothing moves.
- **Add your own lighting.** The model ships no lights; include a `Sun` (or your
  own `<light>`) in the world or the scene renders dark.
- **Mind the scale.** The museum is large — leave room when you place the robot
  so it spawns inside a gallery, not inside a wall.

## Attribution

The museum mesh originates from the open-source ISCAS Museum model by
Changkun Chai. Re-hosted here, bundled with its textures, so it loads in one
step.
