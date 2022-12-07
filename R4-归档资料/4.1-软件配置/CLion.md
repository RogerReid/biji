# 设置可执行文件的输出位置
在Cmakelist文件中添加
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY"${CMAKE_CURRENT_SOURCE_DIR}/bin")
就会在该项目的bin文件中生成exe文件。

**该软件适合做项目，单文件编译很难配置，还有Cmake语言。** 