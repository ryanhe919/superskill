---
name: ci-cd-setup
description: 创建 CI/CD 流水线，支持 GitHub Actions、GitLab CI、Jenkins，包括代码质量检查、测试、构建和部署
---

# CI/CD 流水线设置

你是一个专业的 CI/CD 专家。你的目标是创建高效、可靠的持续集成和持续部署流水线。

## 流水线阶段

### 1. 代码质量检查
- Lint 检查
- 代码格式化验证
- 静态代码分析
- 依赖安全扫描

### 2. 测试
- 单元测试
- 集成测试
- E2E 测试
- 测试覆盖率报告

### 3. 构建
- 编译代码
- 打包应用
- 构建 Docker 镜像
- 生成构建产物

### 4. 部署
- 部署到测试环境
- 部署到预发布环境
- 部署到生产环境
- 回滚机制

### 5. 通知
- 成功/失败通知
- Slack/邮件集成
- 构建状态徽章

## 平台特定配置

### GitHub Actions

```yaml
name: CI/CD 流水线

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20'
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # 代码质量检查
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: 设置 Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: 安装依赖
        run: npm ci

      - name: 代码风格检查
        run: npm run lint

      - name: 类型检查
        run: npm run type-check

      - name: 格式检查
        run: npm run format:check

  # 安全扫描
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: 运行 Snyk 安全扫描
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

      - name: 审计依赖
        run: npm audit --audit-level=moderate

  # 测试
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20]

    steps:
      - uses: actions/checkout@v4

      - name: 设置 Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: 安装依赖
        run: npm ci

      - name: 运行单元测试
        run: npm run test:unit -- --coverage

      - name: 运行集成测试
        run: npm run test:integration

      - name: 上传覆盖率报告
        uses: codecov/codecov-action@v3
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          files: ./coverage/lcov.info

  # 构建
  build:
    needs: [lint, security, test]
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: 设置 Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: 安装依赖
        run: npm ci

      - name: 构建应用
        run: npm run build

      - name: 上传构建产物
        uses: actions/upload-artifact@v3
        with:
          name: build
          path: dist/
          retention-days: 7

  # Docker 构建和推送
  docker:
    needs: [build]
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    permissions:
      contents: read
      packages: write

    steps:
      - uses: actions/checkout@v4

      - name: 设置 Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: 登录容器注册表
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: 提取元数据
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=sha,prefix={{branch}}-
            type=semver,pattern={{version}}

      - name: 构建并推送镜像
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # 部署到测试环境
  deploy-staging:
    needs: [docker]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/develop'
    environment:
      name: staging
      url: https://staging.example.com

    steps:
      - name: 部署到 Staging
        run: |
          echo "部署到测试环境..."
          # 添加部署脚本

      - name: 运行烟雾测试
        run: |
          echo "运行烟雾测试..."
          # 添加测试脚本

  # 部署到生产环境
  deploy-production:
    needs: [docker]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
      url: https://example.com

    steps:
      - name: 部署到生产环境
        run: |
          echo "部署到生产环境..."
          # 添加部署脚本

      - name: 健康检查
        run: |
          echo "执行健康检查..."
          # 添加健康检查脚本

  # 通知
  notify:
    needs: [deploy-staging, deploy-production]
    runs-on: ubuntu-latest
    if: always()

    steps:
      - name: 发送 Slack 通知
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: '部署完成'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

### GitLab CI/CD

```yaml
# .gitlab-ci.yml

variables:
  NODE_VERSION: "20"
  DOCKER_DRIVER: overlay2
  REGISTRY: $CI_REGISTRY
  IMAGE: $CI_REGISTRY_IMAGE

stages:
  - lint
  - test
  - build
  - deploy

# 缓存设置
cache:
  key:
    files:
      - package-lock.json
  paths:
    - node_modules/
    - .npm/

# 代码质量检查
lint:
  stage: lint
  image: node:${NODE_VERSION}
  script:
    - npm ci --cache .npm --prefer-offline
    - npm run lint
    - npm run type-check
    - npm run format:check
  only:
    - merge_requests
    - main
    - develop

# 安全扫描
security:
  stage: lint
  image: node:${NODE_VERSION}
  script:
    - npm ci --cache .npm --prefer-offline
    - npm audit --audit-level=moderate
  allow_failure: true

# 单元测试
test:unit:
  stage: test
  image: node:${NODE_VERSION}
  script:
    - npm ci --cache .npm --prefer-offline
    - npm run test:unit -- --coverage
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
    paths:
      - coverage/
    expire_in: 1 week

# 集成测试
test:integration:
  stage: test
  image: node:${NODE_VERSION}
  services:
    - postgres:15
    - redis:7
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: test_user
    POSTGRES_PASSWORD: test_password
    DATABASE_URL: postgresql://test_user:test_password@postgres:5432/test_db
    REDIS_URL: redis://redis:6379
  script:
    - npm ci --cache .npm --prefer-offline
    - npm run test:integration

# 构建应用
build:
  stage: build
  image: node:${NODE_VERSION}
  script:
    - npm ci --cache .npm --prefer-offline
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week
  only:
    - main
    - develop

# Docker 构建
docker:build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build -t $IMAGE:$CI_COMMIT_SHA .
    - docker tag $IMAGE:$CI_COMMIT_SHA $IMAGE:$CI_COMMIT_REF_SLUG
    - docker push $IMAGE:$CI_COMMIT_SHA
    - docker push $IMAGE:$CI_COMMIT_REF_SLUG
  only:
    - main
    - develop

# 部署到测试环境
deploy:staging:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache curl
  script:
    - echo "部署到测试环境..."
    - curl -X POST $STAGING_DEPLOY_WEBHOOK
  environment:
    name: staging
    url: https://staging.example.com
  only:
    - develop

# 部署到生产环境
deploy:production:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache curl
  script:
    - echo "部署到生产环境..."
    - curl -X POST $PRODUCTION_DEPLOY_WEBHOOK
  environment:
    name: production
    url: https://example.com
  when: manual
  only:
    - main
```

### Jenkins (Jenkinsfile)

```groovy
pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
        REGISTRY = 'docker.io'
        IMAGE_NAME = 'myapp'
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials')
    }

    stages {
        stage('检出代码') {
            steps {
                checkout scm
            }
        }

        stage('设置环境') {
            steps {
                sh """
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
                    export NVM_DIR="\$HOME/.nvm"
                    [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"
                    nvm install ${NODE_VERSION}
                    nvm use ${NODE_VERSION}
                """
            }
        }

        stage('安装依赖') {
            steps {
                sh 'npm ci'
            }
        }

        stage('代码质量') {
            parallel {
                stage('Lint') {
                    steps {
                        sh 'npm run lint'
                    }
                }
                stage('类型检查') {
                    steps {
                        sh 'npm run type-check'
                    }
                }
                stage('安全扫描') {
                    steps {
                        sh 'npm audit --audit-level=moderate'
                    }
                }
            }
        }

        stage('测试') {
            parallel {
                stage('单元测试') {
                    steps {
                        sh 'npm run test:unit -- --coverage'
                    }
                }
                stage('集成测试') {
                    steps {
                        sh 'npm run test:integration'
                    }
                }
            }
            post {
                always {
                    junit 'test-results/**/*.xml'
                    publishCoverage adapters: [coberturaAdapter('coverage/cobertura-coverage.xml')]
                }
            }
        }

        stage('构建') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker 构建') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.build("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                    docker.build("${REGISTRY}/${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Docker 推送') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY}", 'docker-hub-credentials') {
                        docker.image("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                        docker.image("${REGISTRY}/${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('部署') {
            when {
                branch 'main'
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'develop') {
                        sh 'echo "部署到测试环境..."'
                        // 部署脚本
                    } else if (env.BRANCH_NAME == 'main') {
                        input message: '确认部署到生产环境?', ok: '部署'
                        sh 'echo "部署到生产环境..."'
                        // 部署脚本
                    }
                }
            }
        }
    }

    post {
        success {
            slackSend color: 'good', message: "构建成功: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        failure {
            slackSend color: 'danger', message: "构建失败: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        always {
            cleanWs()
        }
    }
}
```

## 最佳实践

1. **快速反馈**: 先运行快速检查（lint），后运行慢速检查（E2E）
2. **并行执行**: 独立任务并行运行以节省时间
3. **缓存依赖**: 缓存 node_modules 等依赖
4. **安全管理**: 使用密钥管理服务存储敏感信息
5. **环境隔离**: 使用不同环境进行测试和部署
6. **自动回滚**: 部署失败时自动回滚
7. **监控告警**: 集成监控和告警系统

开始创建 CI/CD 流水线。
