- **评审意见**：
  1. `.github/workflows/main-local.yml` 文件中注释掉的部分可能不再需要，建议检查是否有必要保留。
  2. `.github/workflows/main-remote-jar.yml` 文件中，描述部分有大量的口语化表达和不必要的解释，建议精简描述以增强可读性。
  3. `wget` 命令下载 JAR 文件时没有指定下载失败时的处理策略。
  4. 在运行 Java JAR 包时，没有进行任何错误处理，如果 JAR 包运行失败，则整个 workflow 也会失败。
  5. 环境变量设置部分较长，可以考虑使用 `.github/workflows/params.yml` 来管理这些参数，使 workflow 更易维护。

- **优化建议**：
  1. 删除或注释掉 `.github/workflows/main-local.yml` 中无用的部分。
  2. 精简 `.github/workflows/main-remote-jar.yml` 文件的描述部分，保留关键信息。
  3. 在下载 JAR 文件时添加错误检查，确保下载成功。
  4. 在运行 JAR 包时添加 try-catch 结构或使用 `exit-code` 来处理可能的错误。
  5. 使用 `.github/workflows/params.yml` 文件来管理环境变量，并在 workflow 中引用它们。

- **优化后代码**：

```yaml
diff --git a/.github/workflows/main-local.yml b/.github/workflows/main-local.yml
index 7f0abbc..c917ebf 100644
--- a/.github/workflows/main-local.yml
+++ b/.github/workflows/main-local.yml
@@ -1,5 +1,5 @@
 name: Run Java Git Diff By Local
-
 # 可废弃
 on:
   push:
     branches:
diff --git a/.github/workflows/main-remote-jar.yml b/.github/workflows/main-remote-jar.yml
new file mode 100644
index 0000000..83c0d7f
--- /dev/null
+++ b/.github/workflows/main-remote-jar.yml
@@ -0,0 +1,73 @@
+name: Build and Run AiCodeReview By Main Remote Jar
+on:
+  push:
+    branches:
+      - master-close
+  pull_request:
+    branches:
+      - master-close
+
+jobs:
+  build:
+    runs-on: ubuntu-latest
+
+    steps:
+      - name: Checkout repository
+        uses: actions/checkout@v2
+        with:
+          fetch-depth: 2
+
+      - name: Set up JDK 11
+        uses: actions/setup-java@v2
+        with:
+          distribution: 'adopt'
+          java-version: '11'
+
+      - name: Create libs directory
+        run: mkdir -p ./libs
+
+      - name: Download ai-code-review-sdk JAR
+        run: |
+          wget -O ./libs/ai-code-review-sdk-1.0.jar https://github.com/RogueYao/ai-code-review-log/releases/download/v1.0/ai-code-review-sdk-1.0.jar
+          if [ $? -ne 0 ]; then
+            echo "Failed to download ai-code-review-sdk-1.0.jar"
+            exit 1
+          fi
+
+      - name: Run Code Review
+        run: java -jar ./libs/ai-code-review-sdk-1.0.jar
+        env:
+          GITHUB_REVIEW_LOG_URI: ${{ secrets.CODE_REVIEW_LOG_URI }}
+          GITHUB_TOKEN: ${{ secrets.CODE_TOKEN }}
+          COMMIT_PROJECT: ${{ env.REPO_NAME }}
+          COMMIT_BRANCH: ${{ env.BRANCH_NAME }}
+          COMMIT_AUTHOR: ${{ env.COMMIT_AUTHOR }}
+          COMMIT_MESSAGE: ${{ env.COMMIT_MESSAGE }}
+          WEIXIN_APPID: ${{ secrets.WEIXIN_APPID }}
+          WEIXIN_SECRET: ${{ secrets.WEIXIN_SECRET }}
+          WEIXIN_TOUSER: ${{ secrets.WEIXIN_TOUSER }}
+          WEIXIN_TEMPLATE_ID: ${{ secrets.WEIXIN_TEMPLATE_ID }}
+          CHATGLM_APIHOST: ${{ secrets.CHATGLM_APIHOST }}
+          CHATGLM_APIKEYSECRET: ${{ secrets.CHATGLM_APIKEYSECRET }}
```

请注意，我没有提供 `.github/workflows/params.yml` 文件的示例，因为这取决于具体的工作流程和参数。如果需要，可以创建该文件并定义环境变量，然后在 workflow 中引用它们。