# 基於 MoveIt2 Jazzy source 版本
FROM moveit/moveit2:jazzy-source

# 設置工作目錄
WORKDIR /root/ws_moveit

# 安裝常用工具
RUN apt-get update && apt-get install -y \
    vim \
    nano \
    htop \
    tree \
    tmux \
    bash-completion \
    python3-pip \
    python3-colcon-common-extensions \
    && rm -rf /var/lib/apt/lists/*

# 提前嘗試 build（若 src 有東西就編，不影響沒有 src 的情況）
RUN /bin/bash -c "source /opt/ros/jazzy/setup.bash && colcon build || true"

# ===== 在 bashrc 建立 alias r =====
RUN echo '' >> ~/.bashrc && \
    echo '# ROS2 & workspace setup' >> ~/.bashrc && \
    echo 'source /opt/ros/jazzy/setup.bash' >> ~/.bashrc && \
    echo 'if [ -f ~/ws_moveit/install/setup.bash ]; then' >> ~/.bashrc && \
    echo '  source ~/ws_moveit/install/setup.bash' >> ~/.bashrc && \
    echo 'fi' >> ~/.bashrc && \
    echo '' >> ~/.bashrc && \
    echo '# Alias: r = build & source workspace' >> ~/.bashrc && \
    echo "alias r='cd ~/ws_moveit && colcon build && source ~/ws_moveit/install/setup.bash'" >> ~/.bashrc

# 預設工作目錄
WORKDIR /root/ws_moveit

# 預設命令
CMD ["/bin/bash"]
