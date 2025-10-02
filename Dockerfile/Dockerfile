# 基於 MoveIt2 Jazzy source 版本
FROM moveit/moveit2:jazzy-source

# 設置工作目錄
WORKDIR /root

# 更新系統包並安裝必要工具
RUN apt-get update && apt-get install -y \
    vim \
    nano \
    htop \
    tree \
    bash-completion \
    && rm -rf /var/lib/apt/lists/*

# 切換到 ws_moveit 目錄
WORKDIR /root/ws_moveit

# 更新 rosdep 數據庫
RUN rosdep update

# 安裝所有依賴
RUN rosdep install --from-paths src --ignore-src -r -y

# 構建所有包
RUN /bin/bash -c "source /opt/ros/jazzy/setup.bash && \
    colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release"

# 創建快捷命令腳本
RUN echo '#!/bin/bash' > /usr/local/bin/r && \
    echo 'cd ~/ws_moveit' >> /usr/local/bin/r && \
    echo 'echo "Building workspace..."' >> /usr/local/bin/r && \
    echo 'source /opt/ros/jazzy/setup.bash' >> /usr/local/bin/r && \
    echo 'colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release' >> /usr/local/bin/r && \
    echo 'if [ $? -eq 0 ]; then' >> /usr/local/bin/r && \
    echo '    echo "Build successful! Sourcing workspace..."' >> /usr/local/bin/r && \
    echo '    source install/setup.bash' >> /usr/local/bin/r && \
    echo '    echo "Workspace ready!"' >> /usr/local/bin/r && \
    echo 'else' >> /usr/local/bin/r && \
    echo '    echo "Build failed!"' >> /usr/local/bin/r && \
    echo 'fi' >> /usr/local/bin/r && \
    chmod +x /usr/local/bin/r

# 創建一個更詳細的重建腳本（rb = rebuild）
RUN echo '#!/bin/bash' > /usr/local/bin/rb && \
    echo 'cd ~/ws_moveit' >> /usr/local/bin/rb && \
    echo 'echo "Cleaning previous build..."' >> /usr/local/bin/rb && \
    echo 'rm -rf build/ install/ log/' >> /usr/local/bin/rb && \
    echo 'echo "Updating dependencies..."' >> /usr/local/bin/rb && \
    echo 'rosdep update' >> /usr/local/bin/rb && \
    echo 'rosdep install --from-paths src --ignore-src -r -y' >> /usr/local/bin/rb && \
    echo 'echo "Building workspace from scratch..."' >> /usr/local/bin/rb && \
    echo 'source /opt/ros/jazzy/setup.bash' >> /usr/local/bin/rb && \
    echo 'colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release' >> /usr/local/bin/rb && \
    echo 'if [ $? -eq 0 ]; then' >> /usr/local/bin/rb && \
    echo '    echo "Build successful! Sourcing workspace..."' >> /usr/local/bin/rb && \
    echo '    source install/setup.bash' >> /usr/local/bin/rb && \
    echo '    echo "Workspace ready!"' >> /usr/local/bin/rb && \
    echo 'else' >> /usr/local/bin/rb && \
    echo '    echo "Build failed!"' >> /usr/local/bin/rb && \
    echo 'fi' >> /usr/local/bin/rb && \
    chmod +x /usr/local/bin/rb

# 設置 bashrc 以自動 source workspace
RUN echo '' >> ~/.bashrc && \
    echo '# MoveIt2 Workspace Setup' >> ~/.bashrc && \
    echo 'source /opt/ros/jazzy/setup.bash' >> ~/.bashrc && \
    echo 'if [ -f ~/ws_moveit/install/setup.bash ]; then' >> ~/.bashrc && \
    echo '    source ~/ws_moveit/install/setup.bash' >> ~/.bashrc && \
    echo 'fi' >> ~/.bashrc && \
    echo '' >> ~/.bashrc && \
    echo '# Useful aliases' >> ~/.bashrc && \
    echo 'alias ws="cd ~/ws_moveit"' >> ~/.bashrc && \
    echo 'alias src="cd ~/ws_moveit/src"' >> ~/.bashrc && \
    echo 'alias ll="ls -la"' >> ~/.bashrc && \
    echo 'alias ..="cd .."' >> ~/.bashrc && \
    echo '' >> ~/.bashrc && \
    echo '# Custom commands info' >> ~/.bashrc && \
    echo 'echo "Custom commands available:"' >> ~/.bashrc && \
    echo 'echo "  r  - Quick rebuild and source workspace"' >> ~/.bashrc && \
    echo 'echo "  rb - Full rebuild (clean + build + source)"' >> ~/.bashrc && \
    echo 'echo "  ws - Go to workspace directory"' >> ~/.bashrc && \
    echo 'echo "  src - Go to src directory"' >> ~/.bashrc

# 設置默認工作目錄
WORKDIR /root/ws_moveit

# 創建一個啟動信息腳本
RUN echo '#!/bin/bash' > /usr/local/bin/info && \
    echo 'echo "=== MoveIt2 Development Container ==="' >> /usr/local/bin/info && \
    echo 'echo "Base image: moveit/moveit2:jazzy-source"' >> /usr/local/bin/info && \
    echo 'echo "Workspace: ~/ws_moveit"' >> /usr/local/bin/info && \
    echo 'echo ""' >> /usr/local/bin/info && \
    echo 'echo "Available packages in workspace:"' >> /usr/local/bin/info && \
    echo 'find src -name "package.xml" -exec dirname {} \; | sed "s|src/||" | sort' >> /usr/local/bin/info && \
    echo 'echo ""' >> /usr/local/bin/info && \
    echo 'echo "Quick commands:"' >> /usr/local/bin/info && \
    echo 'echo "  r   - Rebuild workspace and source"' >> /usr/local/bin/info && \
    echo 'echo "  rb  - Full rebuild (clean first)"' >> /usr/local/bin/info && \
    echo 'echo "  ws  - Go to workspace"' >> /usr/local/bin/info && \
    echo 'echo "  src - Go to src directory"' >> /usr/local/bin/info && \
    echo 'echo "  info - Show this information"' >> /usr/local/bin/info && \
    chmod +x /usr/local/bin/info

# 預先 source workspace
RUN /bin/bash -c "source /opt/ros/jazzy/setup.bash && source install/setup.bash"

# 設置默認命令
CMD ["/bin/bash"]
