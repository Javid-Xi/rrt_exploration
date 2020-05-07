rrt_exploration修改规则

一、single.launch
	eta:此参数控制用于检测边界点的RRT的增长率，单位为米。 此参数应根据地图大小设置，非常大的值将使树生长得更快，从而更快地检测边界点，但是大的增长率也意味着树将缺少地图中的小角落。

	把global_rrt_detector节点的maptopic的value改为/map
	把local_rrt_detector节点的maptopic的value改为/map, robot_frame的value改为/base_link
	把filter.py的maptopic的value改为/map, 把namespace改为<param name="namespace" value=""/> 
	把assigner.py的maptopic的value改为/map, global_frame的value改为/map, namespace改为<param name="namespace" value=""/>

二、global_rrt_detector.cpp
	把整篇出现的ns+都删掉
	把所有的/robot_1删掉
	84行的ns+"_shapes"改为”/local_detected_shapes”

三、local_rrt_detector.cpp
	把整篇出现的ns+都删掉
	把所有的/robot_1删掉
	第85行的ns+"_shapes"改为"/local_detected_shapes"
	第96、97行的mapData.header.frame_id改为”/map”
	整篇出现的map_topic改为”/map”, base_frame_topic改为”/base_link”

四、assigner.py
	整篇出现的map_topic改为”/map”, frontiers_topic改为”/filtered_points”
	最后main下面的的
	try:
        node()
    except rospy.ROSInterruptException:
        pass
 
	改为node()

五、functions.py
	把整篇出现的ns+都删掉
	把整篇出现的self.name+都删掉
	把整篇出现的name+都删掉
	把整篇出现的global_frame和self.global_frame都替换为’/map’
	37、38行的/move_base_node/NavfnROS/make_plan改为/move_base/NavfnROS/make_plan
	删除functions.pyc

六、filter.py
	71行的namespace+str(i+namespace_init_count)+'/move_base_node/global_costmap/costmap'改为'/move_base/global_costmap/costmap'
	73行的'/move_base_node/global_costmap/costmap'改为'/move_base/global_costmap/costmap'
	88行的global_frame[1:], namespace+str(i+namespace_init_count)+'/base_link'改为'/map', '/base_link'
	90行的global_frame[1:], '/base_link'改为'/map', '/base_link'

七、move_base配置文件修改
	这里我们直接用我们下载的仿真包里的move_base的四个配置文件。
	做一下简单的修改costmap_common_params.yaml里把13行的footprint注释掉。
	把29行的sensor_frame: base_laser_link去掉，再把topic：/base_link改为/scan

最后注意：我们这里用的/base_link 是因为我们的机器人的基坐标系是/base_link 但是有的机器人的是/base_footprint 那你就把这个改过来就是了。 
