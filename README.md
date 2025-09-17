# Auto-SAP应用部署说明文档

## 概述

本项目是自动部署argo节点到SAP Cloud Foundry平台，自动保活的方案
- 视频教程：https://www.youtube.com/watch?v=uHvtVaeVCvE
- telegram交流反馈群组：https://t.me/eooceu

### 前置要求
* GitHub 账户：需要有一个 GitHub 账户来创建仓库和设置工作流
* SAP Cloud Foundry 账户：需要有 SAP Cloud Foundry 的有效账户,点此注册：https://www.sap.com

## 部署步骤

1. Fork本仓库

2. 在Actions菜单允许 `I understand my workflows, go ahead and enable them` 按钮

3. 在 GitHub 仓库中设置以下 secrets（Settings → Secrets and variables → Actions → New repository secret）：
- `EMAIL`: Cloud Foundry账户邮箱
- `PASSWORD`: Cloud Foundry账户密码
- `SG_ORG`: 新加坡组织名称
- `US_ORG`: 美国组织名称
- `SPACE`: Cloud Foundry空间名称

4. **设置Docker容器环境变量(也是在secrets里设置)**
   - 使用固定隧道token部署，请在cloudflare里设置端口为8001：
   - 设置基础环境变量：
     - UUID(节点uuid)
     - ARGO_DOMAIN(固定隧道域名,未设置将使用临时隧道)
     - ARGO_AUTH(固定隧道json或token,未设置将使用临时隧道)
     - SUB_PATH(订阅token,未设置默认是sub)
   - 可选环境变量
     - NEZHA_SERVER(v1形式: nezha.xxx.com:8008  v0形式：nezha.xxx.com)
     - NEZHA_PORT(V1哪吒没有这个)
     - NEZHA_KEY(v1的NZ_CLIENT_SECRET或v0的agent密钥)
     - CFIP(优选域名或优选ip)
     - CFPORT(优选域名或优选ip对应端口)

5. **开始部署**
* 在GitHub仓库的Actions页面找到"自动部署SAP"工作流
* 点击"Run workflow"按钮
* 根据需要选择或填写以下参数：
   - environment: 选择部署环境（staging/production）
   - region: 选择部署区域（SG/US）
   - app_name: （可选）指定应用名称
* 点击绿色的"Run workflow"按钮开始部署

6. **获取节点信息**
* 点开运行的actions，点击Deploy application，找到routes: 后面的域名
* 订阅： 域名/$SUB_PATH    SUB_PATH变量没设置默认是sub  即订阅为：域名/sub


## 保活 
* actions保活可能存在时间误差，建议根据前两天的情况进行适当调整`自动保活SAP.yml`里的cron时间
* 推荐使用keep.sh在vps或nat小鸡上精准保活，下载keep.sh文件到本地或vps上，在开头添加必要的环境变量和保活url然后执行`bash keep.sh`即可

## 注意事项

1. 确保所有必需的GitHub Secrets已正确配置
2. 多区域部署需先开通权限，确保US区域有内存
4. 建议设置SUB_PATH订阅token,防止节点泄露
