# 2026 秋冬季开源操作系统训练营 知识库

## GitHub 与 CNB 双向同步

本仓库使用 [tencentcom/git-sync](https://cnb.cool/cnb/plugins/tencentcom/git-sync) 进行 GitHub 与 CNB 双向同步，固定镜像版本为 `v1.3.0`。下面给出配置步骤，作为备忘。

### 配置 GitHub → CNB

打开 GitHub 仓库配置的“Secrets and variables → Actions”，添加以下项目：

| 类型 | 名称 | 值 |
| --- | --- | --- |
| Secret | `CNB_ACCESS_TOKEN` | CNB 访问令牌（在 CNB「个人设置 → 访问令牌」创建令牌） |
| Variable | `CNB_REPO_URL` | CNB 仓库地址：`https://cnb.cool/<组织>/<仓库>.git` |

### 配置 CNB → GitHub

1. 在 GitHub 创建有权访问本仓库的 Fine-grained Personal Access Token，授予 `Contents: Read and write` 和 `Workflows: Read and write` 权限。

2. 在 CNB 创建一个[**密钥仓库**](https://cnb.cool/Joint-Discovery-Zone/secrets)，通过其网页编辑器新建密钥文件（例如 `sync-to-github-jeffrey-gong.yml`）：

```yaml
allow_slugs:
  - "<CNB 代码仓库的完整组织/仓库路径，不含域名和 .git>"
allow_events:
  - push
allow_branches:
  - main
allow_images:
  - tencentcom/git-sync:v1.3.0

GIT_USERNAME: "<GitHub 令牌所属用户名>"
GIT_ACCESS_TOKEN: "<GitHub Personal Access Token>"
```

3. 复制该密钥文件的网页链接，替换 CNB 配置（`.cnb.yml`）中的 `imports` 导入地址，完整格式为 `https://cnb.cool/<密钥仓库路径>/-/blob/<分支>/<文件路径>`。
