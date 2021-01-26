# lxl-admin edit
## 对项目升级
1. 对 vue项目升级
   ``` vue upgrade ```
2. 安装 npm-check-updates 检查依赖包的升级
  ```
    ncu: 查看 项目可更新的包状态
    ncu -u: 开始更新
    注意:
      less 和 less-loader 不需要去更新 依赖旧版本,  echarts用 v4 版本
      富文本wangeditor 最新v4版本,不需要单独引入 css,  V4 和 V3 的配置和使用极为相似（把 editor.customConfig 改为 editor.config 即
  ```
