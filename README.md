# WaitRecovery

A recovery behavior for move_base which when executed waits for specified time. 

## Parameters

`~wait_duration(double, default:5.0)`
&emsp;&emsp;The time duration in seconds the robot should wait before executing next step.

## Usage

clone and build the package within your workspace,

Verify the availability of the package,
```
rospack plugins --attrib=plugin nav_core
```

Following line should be present within the response with path to your workspace,
```
wait_recovery /<path_to_ws>/src/wait_recovery/wait_plugin.xml
```

Move base needs to be registered to use this plugin, a list containing recovery behaviors to be used should be passed as a parameter. To set the params within the WaitRecovery behavior a `.yaml` file can be loaded.


```
<launch>

    <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="screen">
     <!-- Other ros params and remaps -->

      <rosparam file="$(find <your_package>)/param/recovery_params.yaml" command="load" />

        <param name="recovery_behaviors" value="[
                {name: conservative_reset, type: clear_costmap_recovery/ClearCostmapRecovery}, 
                {name: wait, type: wait_recovery/WaitRecovery}
            ]" type="yaml" />
    
    </node>
  </launch>
```

Contents within the `.yaml` file,

```
# Wait

wait_duration: 10.0
```


## Why Wait Recovery?

Any dynamic obstacle which appears in the vicinity of the robot leads to failure of DWAPlanner to follow the global path. A good recovery behavior entails a brief pause, allowing time for dynamic obstacles to potentially clear on their own.