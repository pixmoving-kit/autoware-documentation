<a id="installation-of-related-tools"></a>

# 安装相关工具

<a id="calibration-tools"></a>

## 标定工具

<a id="calibration-tools-provided-by-tier-iv"></a>

### TIER IV 提供的标定工具

安装 Autoware 后，可以按以下方式安装 TIER IV 提供的标定工具：

```bash
cd autoware
wget https://raw.githubusercontent.com/tier4/CalibrationTools/refs/heads/tier4/universe/calibration_tools_autoware.repos
vcs import src < calibration_tools.repos
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

更详细的说明请参阅 [README.md 文件](https://github.com/tier4/CalibrationTools/blob/tier4/universe/README.md)。
