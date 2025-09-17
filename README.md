# Auto-SAP应用部署说明文档

### 前置要求
* GitHub 账户：需要有一个 GitHub 账户来创建仓库和设置工作流
* SAP Cloud Foundry 账户：需要有 SAP Cloud Foundry 的有效账户,点此注册：https://www.sap.com

## 部署步骤

1. 在 GitHub 仓库中设置以下 secrets（Settings → Secrets and variables → Actions → New repository secret）：
- `EMAIL`: Cloud Foundry账户邮箱
- `PASSWORD`: Cloud Foundry账户密码
- `SG_ORG`: 新加坡组织名称
- `US_ORG`: 美国组织名称
- `SPACE`: Cloud Foundry空间名称

2. **开始部署**
* 在GitHub仓库的Actions页面找到"自动部署SAP"工作流
* 点击"Run workflow"按钮
* 根据需要选择或填写以下参数：
   - environment: 选择部署环境（staging/production）
   - region: 选择部署区域（SG/US）
   - app_name: （可选）指定应用名称
* 点击绿色的"Run workflow"按钮开始部署

## 保活 
* actions保活可能存在时间误差，建议根据前两天的情况进行适当调整`自动保活SAP.yml`里的cron时间
* 推荐使用keep.sh在vps或nat小鸡上精准保活，下载keep.sh文件到本地或vps上，在开头添加必要的环境变量和保活url然后执行`bash keep.sh`即可

## 注意事项

1. 确保所有必需的GitHub Secrets已正确配置
2. 多区域部署需先开通权限，确保US区域有内存
4. 建议设置SUB_PATH订阅token,防止节点泄露
