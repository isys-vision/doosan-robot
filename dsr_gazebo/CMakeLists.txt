cmake_minimum_required(VERSION 3.22.0)
project(dsr_gazebo)
find_package(catkin REQUIRED)

catkin_package()

install(
	DIRECTORY config launch worlds
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
