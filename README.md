# DalamudPluginsD17

你好！ 这是 [Final Fantasy XIV 的 Dalamud 插件框架](https://github.com/ottercorp/Dalamud) 的插件库。 此存储库是 [DalamudPlugins](https://github.com/ottercorp/DalamudPlugins) 的继承者并实现了 [DIP17](https://github.com/goatcorp/DIPs/blob/main/text/17-automated-build-and-submit-pipeline.md) 使提交过程更容易和更快。

## 发布你的插件

### 准备你的仓库

- 确保您的插件位于可公开访问的 Git 存储库中（GitHub、GitLab、任何允许 HTTP 克隆而无需身份验证的自托管 Git 实例）
- 更新你的 .csproj
   - 如果您还没有使用 `$(DalamudLibPath)`，请参阅 <https://github.com/goatcorp/SamplePlugin/blob/c6a5f5fcbf8e6812f274fab6347307c0283bd6fb/SamplePlugin/Dalamud.Plugin.Bootstrap.targets#L10>
- 在 Release 中构建你的插件，提交你的 `.csproj` + 新生成的 lock 文件

### 检查标准

当插件检查组检查你的插件时，他们将检查以下内容。

- 它是否符合由小组的多个成员商定的[我们的准则](https://ottercorp.github.io/faq/development#q-%E6%88%91%E5%8F%AF%E4%BB%A5%E5%9C%A8%E6%88%91%E7%9A%84%E6%8F%92%E4%BB%B6%E4%B8%AD%E5%81%9A%E4%BB%80%E4%B9%88), as agreed upon by multiple members of the group？
- 它是否具包含战斗相关功能？如果有，它们是否纯粹是信息性的，并且只显示玩家通常会知道的信息？
- 它是否通过了非正式的代码审查？
- 它是否安装得很干净？
- 配置窗口（如果有的话）的行为是否正确？
- 插件的基本功能是否正常（如果可以轻松测试）？
- 它没有明显的技术问题吗？
- 它的JSON格式是否正确？(我们希望[在未来使之成为不必要的](https://github.com/goatcorp/DalamudPackager/issues/8))
- 如果它是一个新的插件，它是否在测试频道而不是稳定频道？如果它是一个简单的插件，或者你已经单独测试过了，你可能可以跳过测试阶段 - 请在你的PR中写上一些细节，或者联系我们!
- 它是否符合[技术标准](#技术标准)？

这些标准是为了防止用户出现问题。我们很乐意与你合作，可以在讨论区联系。

### 技术标准

在这里提交你的插件之前，你应该做一些技术性的工作。它们会使你的插件使用起来更顺手。

- 你的插件必须有一个`icon.png`，在`images/`中不大于512x512，不小于64x64。
- 对于普通的ImGui窗口，如设置和实用窗口，你应该使用[Dalamud Windowing API](https://goatcorp.github.io/Dalamud/api/Dalamud.Interface.Windowing.html)。它增强了窗口的一些很好的功能，like integration into the native UI closing-order, pinning, and opacity controls. If it looks like a window, it should use the windowing API. We won't reject updates to existing plugins for this, but we encourage everyone to upgrade.
- Your plugin's version/assembly version **must not** be based on a timestamp or continually increasing build number. Every time your plugin is built with a specific commit, no matter the time or date, should produce the same version.

### 提交

- Fork 此存储库，或使用 GitHub 网络编辑器（在存储库中按 `.`，或按现有清单上的 ✏ 图标）
- 在你的 fork 中，制作 `stable/(plugin name)/manifest.toml`（或 `testing/live/(plugin name)/manifest.toml` - 请注意，我们更喜欢新插件进入 `testing/live`， 以便在向更广泛的受众传播之前解决皱纹问题）。 有关更多信息，[参见此处](https://github.com/goatcorp/DIPs/blob/main/text/17-automated-build-and-submit-pipeline.md#guide-level-explanation)。
- 请注意，请确保您的用户名在 `owners` 中，即便您只是将插件进行了国服适配（例如下面的`owners = ["goaaats", "Bluefissure"]`）。

  ```toml
  [plugin]
  repository = "https://github.com/goatcorp/SamplePlugin.git"
  commit = "765d9bb434ac99a27e9a3f2ba0a555b55fe6269d"
  owners = ["goaaats", "Bluefissure"]
  project_path = "SamplePlugin"
  changelog = "Added Herobrine"
  ```
  
- 将插件的图像放在 `images` 子文件夹中：`stable/（插件名称）/images`。
   - 请注意，这将 [在未来的某个时候进行简化](https://github.com/goatcorp/DIPs/pull/45)。 这尚未[实施](https://github.com/goatcorp/DalamudPackager/issues/9)。 如果您能提供帮助，我们很乐意听取您的意见！
- 创建 PR。 如果您使用的是 GitHub 网络编辑器，这将是自动的。

您还需要使用 DalamudPackager； 请查看 SamplePlugin 示例。 如果您需要帮助，请联系我们。

## 更新你的插件

只需编辑清单中的提交 commit 的哈希（sha256）。 请始终从新分支进行更新，以便我们进行更清晰的审核。

## 在 PR 中重建

如果你想触发你的 PR 的重建，只需发表一条内容为 `@aonyxbot rebuild` 的评论。

## Secrets

If your build process requires secrets, or you want to include a secret in your plugin, use [this page](https://ottercorp.github.io/plogon-secrets/) to encrypt the secret, to be included via your manifest. It will then be made available to your plugin's MSBuild/build script via environment variables, as per the instructions on the page.

---

提交插件时，请考虑我们的 [可接受使用政策](<https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Acceptable-Use-Policy-(Official-Plugin-Repository)>) 和 [服务条款](<https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Terms-and-Conditions-of-Use-(XIVLauncher,-Dalamud-&-Official-Plugin-Repository)>)，例如详细说明将插件上传到此存储库时您需要授予我们的权利。
请查看 [插件收养政策](https://github.com/goatcorp/faq/blob/main/development.md#adoption) 以了解如果您放弃插件会发生什么。 常见问题解答还提供了有关在从其他开发人员接管时如何提交插件的说明。
