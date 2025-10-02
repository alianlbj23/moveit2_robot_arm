# MoveIt2 開發環境使用說明

## 概述
這個環境基於 `moveit/moveit2:jazzy-source`，預先構建了 MoveIt2 相關包，並提供了便捷的開發工具。

## 快速開始

### 1. 構建和啟動容器
```bash
./activate_moviet.sh
```

### 2. 容器內可用的快捷命令

- **`r`** - 快速重建和 source workspace
  ```bash
  r
  ```

- **`rb`** - 完整重建（清理 + 構建 + source）
  ```bash
  rb
  ```

- **`ws`** - 切換到 workspace 目錄
  ```bash
  ws
  ```

- **`src`** - 切換到 src 目錄
  ```bash
  src
  ```

- **`info`** - 顯示環境信息和可用包
  ```bash
  info
  ```

## 環境特性

### 預配置內容
- 基於 ROS2 Jazzy
- 預安裝 MoveIt2 相關包（moveit_msgs, moveit2）
- 自動配置環境變量
- 預先構建所有包

### 目錄結構
```
~/ws_moveit/
├── src/           # 您的源代碼（從主機掛載）
├── build/         # 構建文件
├── install/       # 安裝文件
└── log/           # 構建日志
```

### 開發工作流

1. **修改代碼**：在主機上編輯 `src/` 目錄下的文件
2. **重建包**：在容器內運行 `r` 命令
3. **測試**：運行您的 ROS2 節點和啟動文件

## Docker 管理

### 使用 Docker Compose（推薦）
```bash
# 構建映像
docker-compose build

# 啟動開發環境
docker-compose run --rm moveit_dev

# 清理
docker-compose down -v
```

### 直接使用 Docker
```bash
# 構建映像
docker build -t moveit-dev:jazzy ./Dockerfile/

# 啟動容器
docker run -it --rm \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
    -v $(pwd)/src:/root/ws_moveit/src \
    --network host \
    moveit-dev:jazzy
```

## 故障排除

### GUI 應用程序無法顯示
確保 X11 轉發已啟用：
```bash
xhost +local:docker
```

### 構建失敗
1. 檢查依賴：容器內運行 `rosdep install --from-paths src --ignore-src -r -y`
2. 清理重建：運行 `rb` 命令
3. 查看詳細錯誤：檢查 `log/` 目錄

### 包找不到
確保您的包已正確添加到 `src/` 目錄並運行了 `r` 命令重建。

## 自定義

### 添加新的工具或包
編輯 `Dockerfile/Dockerfile` 並重建映像：
```bash
docker-compose build --no-cache
```

### 修改快捷命令
快捷命令腳本位於容器的 `/usr/local/bin/` 目錄下，可以在 Dockerfile 中修改。# moveit2_robot_arm
