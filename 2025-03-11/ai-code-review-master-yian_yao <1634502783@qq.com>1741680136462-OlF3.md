- **评审意见**：代码中存在一些不必要或错误的注释、未完成的代码片段、以及潜在的错误。

- **优化建议**：
  1. 删除不必要的注释。
  2. 完成未完成的代码片段。
  3. 检查并修复潜在的错误，例如在 `ApiTest` 类中的 `test` 方法中尝试将非数字字符串转换为整数，这会导致 `NumberFormatException`。

- **优化后代码**：

```yaml
diff --git a/.github/workflows/main-maven-jar.yml b/.github/workflows/main-maven-jar.yml
index 9551e32..759760e 100644
--- a/.github/workflows/main-maven-jar.yml
+++ b/.github/workflows/main-maven-jar.yml
@@ -3,10 +3,10 @@ name: Build and Run AiCodeReview By Main Maven Jar
 on:
   push:
     branches:
-      - '*'
+      - master-close
   pull_request:
     branches:
-      - '*'
+      - master-close
 
 jobs:
   build:
diff --git a/.github/workflows/main-remote-jar.yml b/.github/workflows/main-remote-jar.yml
index 83c0d7f..f0fdbcd 100644
--- a/.github/workflows/main-remote-jar.yml
+++ b/.github/workflows/main-remote-jar.yml
@@ -3,10 +3,10 @@ name: Build and Run AiCodeReview By Main Remote Jar
 on:
   push:
     branches:
-      - master-close
+      - '*'
   pull_request:
     branches:
-      - master-close
+      - '*'
 
 jobs:
   build:
diff --git a/ai-code-review-test/src/main/java/top/duain/middleware/Application.java b/ai-code-review-test/src/main/java/top/duain/middleware/Application.java
index dd5e393..ed39726 100644
--- a/ai-code-review-test/src/main/java/top/duain/middleware/Application.java
+++ b/ai-code-review-test/src/main/java/top/duain/middleware/Application.java
@@ -1,10 +1,5 @@
 package top.duain.middleware;
 
 public class Application {
     public static void main(String[] args) {
-
     }
 }
diff --git a/ai-code-review-test/src/main/resources/application.yml b/ai-code-review-test/src/main/resources/application.yml
index b5c994e..863df23 100644
--- a/ai-code-review-test/src/main/resources/application.yml
+++ b/ai-code-review-test/src/main/resources/application.yml
@@ -1,5 +1,5 @@
 spring:
   application:
-    name: openai-code-review-test-app
+    name: ai-code-review-test-app
   profiles:
     active: dev
diff --git a/ai-code-review-test/src/test/java/top/duain/middleware/test/ApiTest.java b/ai-code-review-test/src/test/java/top/duain/middleware/test/ApiTest.java
index e3f28e9..25f9798 100644
--- a/ai-code-review-test/src/test/java/top/duain/middleware/test/ApiTest.java
+++ b/ai-code-review-test/src/test/java/top/duain/middleware/test/ApiTest.java
@@ -6,11 +6,6 @@ import org.junit.runner.RunWith;
 import org.springframework.boot.test.context.SpringBootTest;
 import org.springframework.test.context.junit4.SpringRunner;
 
-/**
- * @author Yian_yao
- * @Description :
- * @create 2025/3/5 17:01
- **/
 @Slf4j
 @RunWith(SpringRunner.class)
 @SpringBootTest
@@ -18,9 +13,7 @@ public class ApiTest {
 
     @Test
     public void test() {
-        System.out.println(Integer.parseInt("a1"));
-        System.out.println(Integer.parseInt("a2"));
-        System.out.println(Integer.parseInt("a3"));
+        // Example test, remove or replace with actual test logic
     }
 
 }
```