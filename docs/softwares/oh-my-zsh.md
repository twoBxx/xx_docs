# oh my zsh 安装配置  基于 Centos7

## 安装
### 前置：**git** && **zsh**
1. 安装zsh（若有可跳过），执行命令：`yum install -y zsh`
2. 安装git（若有可跳过），执行命令：`yum install -y git`
3. 安装oh my zsh（支持2种方式安装），执行命令（二选一即可）：
    1. `sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"`
        2. `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

        ## 配置

        ### 主题配置
        oh my zsh 内置很多主题，也可[点击查看](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)更多主题
        推荐使用  **agnoster** 主题
        此主题需要字体支持，执行命令：
        1. `git clone https://github.com/powerline/fonts.git`
        2. `sh fonts/install.sh`

        ### 插件配置

        <p style="color:red">所有已安装插件都需要在>所有已安装插件都需要在 `.zshrc` 文件中配置 `plugins=(a b c *)` 以空格隔开</p>

        #### [autojump](https://github.com/wting/autojump "autojump官网")  路径跳转

        > 使用 autojump 的缩写 `j` , `cd` 命令进入 `/etc/nginx` 文件夹，下一次再想进入 nginx 文件夹的时候,直接 `j nginx` 即可
        或者只输入 `nginx` 的一部分 `ngi` 都行
        > git 安装 `git clone git://github.com/joelthelion/autojump.git`,进入目录执行./install.py
        > 在 `~/.zshrc` 中加入 `[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh`

        #### [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting "官网")  命令高亮

        ```shell
        #安装
        git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
        # 配置 .zshrc  plugins=(... zsh-syntax-highlighting)
        ```

        #### [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions "官网") 命令建议
        ```shell
        #安装
        git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
        # 配置 .zshrc  plugins=(... zsh-autosuggestions)
        ```






