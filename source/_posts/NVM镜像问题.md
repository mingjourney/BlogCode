### NVM镜像问题

**查看 `NVM_DIR` 环境变量**：

- 打开终端并运行以下命令：

  ```
  bash
  复制代码
  echo $NVM_DIR
  ```

- 这将显示 NVM 的安装路径，通常是 `~/.nvm`。

**查看 NVM 的配置文件**：

- NVM 的设置通常是在你的 shell 配置文件中定义的，例如 

  ```
  ~/.bashrc
  ```

  、

  ```
  ~/.bash_profile
  ```

  （对于 Bash）或 

  ```
  ~/.zshrc
  ```

  （对于 Zsh）。打开这些文件并查找与 NVM 相关的行，例如：

  ```
  bash
  复制代码
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
  ```

- 这里的 `NVM_DIR` 就是 NVM 的安装路径，而 `nvm.sh` 是 NVM 的主脚本，用于加载 NVM 功能。

**查看 NVM 版本**：

- 你可以通过以下命令查看 NVM 的版本以及其他设置：

  ```
  bash
  复制代码
  nvm --version
  ```

- 你也可以使用 `nvm debug` 查看更详细的 NVM 环境信息，包括当前使用的 Node 版本、NVM 目录等。

**自定义 NVM 设置**：

- 如果你想自定义 NVM 的设置，可以在你的 shell 配置文件中添加或修改相关环境变量。例如，设置默认 Node 版本：

  ```
  bash
  复制代码
  nvm alias default node
  ```

- 保存文件后，记得运行以下命令使其生效：

  ```
  bash
  复制代码
  source ~/.bashrc   # 如果你使用的是 Bash
  source ~/.zshrc    # 如果你使用的是 Zsh
  ```