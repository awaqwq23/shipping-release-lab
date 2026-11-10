# Shipping Release Lab：1.3.0

本工程是第 8 次课的运费计算实验。使用 JDK 21、Maven 3.9+ 和 Git。

## 运行规则

STANDARD 订单满 199.00 元免运费，VIP 订单满 99.00 元免运费，其余收取 15.00 元。两个门槛都包含等于。命令行支持非负金额及大写 STANDARD、VIP 两种类型，不包含折扣、税费、数据库或商城功能。

## 构建与运行

在本文件和 pom.xml 所在目录打开 PowerShell：

```powershell
java -version
mvn -version
mvn -B clean verify
java -cp target/shipping-release-lab-1.3.0.jar edu.course.shipping.Main 199.00 STANDARD
java -cp target/shipping-release-lab-1.3.0.jar edu.course.shipping.Main 99.00 VIP
java -cp target/shipping-release-lab-1.3.0.jar edu.course.shipping.Main 98.99 VIP
```

三次输出依次为 shipping=0.00、shipping=0.00、shipping=15.00。首次构建需要下载依赖。发布附件位于相邻 release 目录，可以把命令中的 JAR 路径改为 ../release/shipping-release-lab-1.3.0.jar 直接运行。

## 本次修改

ShippingRiskTest 新增 STANDARD 与 VIP 各三个门槛测试，以及 new String("VIP") 在 120.00 元时的反例，共 7 次。原有 ShippingCalculatorTest 的 3 次测试及 assertFee 辅助方法保留。新增测试在修复前抓住 3 个失败，修复后 10 次全部通过。

ShippingCalculator 用 equals 比较客户类型内容，两处 compareTo 判断改为 >= 0。版本信息同步到 pom.xml、CHANGELOG.md 和本文件。Main.java 的临时规范违规已恢复。

## 默认质量检查

verify 默认执行 JUnit、Checkstyle、JaCoCo 和 SpotBugs，不需要 profile。Checkstyle 检查 main/test Java 中的 Tab 和行尾空白；JaCoCo 对 ShippingCalculator 要求行覆盖率至少 90%、分支覆盖率至少 80%；SpotBugs 分析计算器的可疑缺陷模式。检查通过只覆盖各自的检查范围。

报告由构建生成，位于 target/surefire-reports、target/checkstyle-result.xml、target/site/jacoco/index.html、target/spotbugsXml.xml。覆盖率页面第一个 Cov. 是指令覆盖率；行覆盖率应由计算器类 Total 行的 Lines 和对应 Missed 计算。

## 版本与提交内容

仓库保留起始 main、feature/vip-shipping 和 v1.2.0 历史，修复提交在 feature 分支，个人复核后 Squash 到 main，正式版本由附注标签 v1.3.0 标记。发布记录在仓库外 ../release/RELEASE_RECORD.md，包含完整提交 SHA、JAR 摘要和实际运行结果。

提交保留隐藏 .git、源码、测试、配置和文档，排除 target 及 IDE 生成目录。preflight.cmd、preflight.ps1 只适用于未修改的教师起始包，完成工程使用 mvn -B clean verify。在线 GiteeGo 属于独立扩展，本地版本没有配置或运行在线 CI。
