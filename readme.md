# sum4all 插件 (增强版)

这是 `sum4all` 插件的一个修改版本，旨在提供更灵活、更广泛的大语言模型（LLM）支持。

## 主要修改点 ✨

1.  **移除原版 `sum4all` API 依赖:**
    *   不再依赖原版的 `pro.sum4all.site` 服务。
    *   总结和搜索功能现在直接调用您在主配置文件 (`config.json`) 中配置的各大模型 API。

2.  **新增大模型支持:**
    *   ✅ **阿里云 百炼 (Aliyun Tongyi):** 添加了 `aliyun` 作为 `service` 选项。
    *   ✅ **硅基流动 (SiliconFlow):** 添加了 `sflow` 作为 `service` 选项。
    *   **模型细分:** 支持为不同任务配置特定模型（例如，在 `keys` 配置中分别设置 `aliyun_sum_model`, `aliyun_vl_model`, `siliconflow_sum_model`, `siliconflow_vl_model` 等）。

3.  **优化微信公众号文章总结:**
    *   针对微信公众号链接，增加了特殊的网页内容获取逻辑。
    *   尝试使用 `requests` 库直接请求，并结合 `BeautifulSoup4` (bs4) 解析页面内容，以提高在遇到反爬虫限制时的成功率。
    *   **注意:** 此方法经测试有效，但不能保证 100% 成功。如果仍然遇到反爬警告或无法获取内容，可能与服务器 IP 地址有关。

## 配置指南 ⚙️

1.  **获取 API 密钥:** 前往阿里云、硅基流动或其他您想使用的 LLM 服务提供商官网获取 API Key 和 API Base URL。

2.  **主配置文件 (`config.json`):**
    *   在根目录的 `config.json` 文件的 `"keys"` 部分，添加对应服务的配置项。请参考本插件目录下的 `config.json.template` 文件作为示例，例如添加：
      ```json
      "keys": {
          // ... other keys ...
          "aliyun_key": "sk-your_aliyun_key",
          "aliyun_base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1",
          "aliyun_sum_model": "qwen-long",
          "aliyun_vl_model": "qwen-vl-max",
          "siliconflow_key": "sk-your_siliconflow_key",
          "siliconflow_base_url": "https://api.siliconflow.com/v1",
          "siliconflow_sum_model": "Qwen/Qwen2.5-72B-Instruct", // 示例
          "siliconflow_vl_model": "Qwen/Qwen2.5-VL-72B-Instruct" // 示例
      }
      ```
    *   在根目录 `config.json` 中，找到 `"url_sum"`, `"file_sum"`, `"image_sum"` 等功能配置块。
    *   将对应功能的 `"enabled"` 设置为 `true`。
    *   将 `"service"` 设置为您想要使用的服务名称，例如 `"aliyun"` 或 `"sflow"`。
      ```json
      "url_sum": {
          "enabled": true,
          "service": "sflow", // 或 "aliyun", "openai", "gemini", "azure"
          // ... other url_sum settings ...
      },
      "file_sum": {
          "enabled": true,
          "service": "aliyun", // 或 "sflow", "openai", "gemini", "azure"
          // ... other file_sum settings ...
      },
      "image_sum": {
          "enabled": true,
          "service": "aliyun", // 或 "sflow", "openai", "gemini", "azure"
          // ... other image_sum settings ...
      }
      ```

3.  **模板文件:** 本插件目录下的 `config.json.template` 文件提供了更详细的配置项说明和示例，请务必参考。

## 使用

配置完成后，根据主配置文件中设置的前缀（如分享链接、发送文件/图片，或使用 `问`、`搜` 等前缀）即可触发相应的功能。

## 支持与反馈 ⭐

*   如果您希望此插件支持其他大模型或有新的功能需求，欢迎提交 **Issue**。
*   如果您觉得这个插件（的修改）对您有帮助，请不吝点亮旁边的 **Star** 哦！