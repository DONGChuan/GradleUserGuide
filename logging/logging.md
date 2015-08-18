# Logging

Log 是构建的主要"UI"工具. 如果日志太过冗长, 那么真正的警告和问题会隐藏其中, 另一方面, 如果你出错了,你又需要搞清楚相关错误信息. Gradle 提供了6个等级的 log, 如[表17.1.Logs Level]()所示.出了那些你可能经常看到的, 还有两个是 Gradle 特定级别的日志,被称为*QUIET*和*LIFECYCLE*.后者是默认的, 并用于报告生成进度.

**表17.1.Logs Level**

| Level | 用途 |
| -- | -- |
| ERROR | 错误信息 |
| QUIET | 重要消息信息 |
| WARNING | 警告信息 |
| LIFECYCLE | 进度消息信息 |
| INFO | 信息消息 |
| DEBUG | 调试信息 |


