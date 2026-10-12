---
title: "Spotify 快捷指令的实现原理与使用指南"
date: 2026-10-12
lastmod: 2026-10-12
author: ["沧海"]
tags: ["iOS", "Spotify"]
description: ""
comments: true
showToc: true
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

在 iOS「快捷指令」App 中搜索 Spotify，无法检索到任何原生动作。Spotify 官方至今未向快捷指令适配 App Intent 接口，但社区中可以通过调用历史意图记录，实现轻点即可后台播放的控制效果。

本文通过对 `.shortcut` 实体文件的二进制逆向分析，详细拆解其背后的 SiriKit 意图捐赠与重放机制，剖析「单动作恢复播放」与包含 13 个动作的「点播合集」的底层数据差异，并提供实体文件与自动化配置方法。

> 快捷指令文件获取

- **单动作恢复播放**：[Play_Media.shortcut](https://cdno.canghai.org/shortcuts/Play_Media.shortcut)
- **13 动作点播合集**：[Spotify_Actions_List.shortcut](https://cdno.canghai.org/shortcuts/Spotify_Actions_List.shortcut)

**导入方式**：在 iPhone 上使用 Safari 浏览器打开链接下载，然后在系统自带的「文件」App 中点击下载好的 `.shortcut` 文件，系统将自动唤起「快捷指令」App 并提示添加。

> 核心原理：SiriKit 意图捐赠与重放

Spotify 官方仅接入了苹果的 **SiriKit Media Intent**，供用户通过语音指令点歌（例如「Hey Siri, play music on Spotify」）。

iOS 系统具备一套意图捐赠机制（Intent Donation）：当用户通过 Siri 或 App 完成某项操作后，系统会将该操作抽象为一个 Intent 对象并记录在系统数据库中，以便在 Spotlight 搜索与快捷指令的建议流中适时推荐。

在 iOS 17 及更早系统中，当用户多次通过 Siri 触发 Spotify 播放后，快捷指令的动作建议库中便会出现对应的建议项。将其单独导出为动作后，其底层标识符为：

```text
is.workflow.actions.sirikit.donation.handle
```

该动作的执行逻辑为**重放（Replay）**：直接向系统提交一份已录制完成的 `_INPBPlayMediaIntent` 协议缓冲区（Protobuf）数据，驱动系统按照路由将意图下发给目标 App 执行。

此外，动作参数中包含：

```text
supportsBackgroundExecution = true
```

该标记允许系统在后台静默完成意图派发与执行，无需唤起 Spotify 的前台界面。

> 单动作恢复播放（Play Media）底层拆解

将 `Play_Media.shortcut` 文件反编译为属性列表（bplist），其核心为一个单一动作：

```json
{
  "WFWorkflowMinimumClientVersion": 900,
  "WFWorkflowClientVersion": "3612.0.1.3",
  "WFWorkflowTypes": ["Watch"],
  "WFWorkflowActions": [
    {
      "WFWorkflowActionIdentifier": "is.workflow.actions.sirikit.donation.handle",
      "WFWorkflowActionParameters": {
        "IntentData": "(3013 字节二进制数据)",
        "ForceExecutionOnPhone": false,
        "UUID": "846435B6-889E-49AB-A4CC-72E4C6396C87",
        "ShowWhenRun": false
      }
    }
  ]
}
```

`IntentData` 字段长度为 3013 字节，由 `NSKeyedArchiver` 序列化封装。解包后提取核心字段：

1. **路由目标**：
   ```text
   x-apple-siri-app://com.spotify.client
   ```
   标识意图的接收进程为 Spotify 客户端。
2. **捐赠记录标识**：
   ```text
   B29B5A35-858D-437D-9CF9-9EDF480D6D21
   ```
   流传的单动作指令均保持该 UUID 一致，源自同一次生成的捐赠记录。
3. **Protobuf 核心载荷**：
   提取 `backingStore` 中的原始 Protobuf 字节，总长度仅有 121 字节，其包含的可读字符串为：
   ```text
   "Play"
   "x-apple-siri-app://com.spotify.client"
   "Play Spotify"
   ```

**数据特性与运行逻辑**：
该载荷中**不包含任何歌曲、歌单 URI，也不包含任何个人账号信息与访问令牌（Token）**。
当系统将该意图投递给 Spotify 进程时，Spotify 接收到的是一条无指定目标的播放意图，客户端内部逻辑会默认执行**恢复上次播放状态（Resume Last Playback）**。正因其完全不依赖外部参数与鉴权 Token，该指令在不同设备和账号之间具有通用性。

> 13 动作点播合集（Spotify Actions List）深度剖析

社区中另一份广泛流传的快捷指令包含了 13 个独立动作。对其解包分析后，可以发现内部包含了多种截然不同的控制机制：

| 序号 | 动作名称 | 控制目标 / URI | 播放模式 | 底层机制 |
| :--- | :--- | :--- | :--- | :--- |
| 0 | Your Liked Songs | `spotify:user:%40:collection` | 随机播放 | SiriKit 意图重放 (`PLAY_MYTRACKS`) |
| 1 | Today’s Top Hits | `spotify:playlist:37i9dQZF1DXcBWIGoYBM5M` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 2 | Viral Hits | `spotify:playlist:37i9dQZF1DX2L0iB23Enbq` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 3 | Hot Hits USA | `spotify:playlist:37i9dQZF1DX0kbJZpiYdZl` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 4 | Discover Weekly | `spotify:playlist:37i9dQZEVXcNti4VTWwM3C` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 5 | Daily Mix 1 | `spotify:playlist:37i9dQZF1E36pkFxRA7iOJ` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 6 | Daily Mix 2 | `spotify:playlist:37i9dQZF1E38yU6utqUgr5` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 7 | Daily Mix 3 | `spotify:playlist:37i9dQZF1E38wfI1xbfRBO` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 8 | Daily Mix 4 | `spotify:playlist:37i9dQZF1E37yoEQV9IWUf` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 9 | Brown Noise (Shuffle) | `spotify:playlist:37i9dQZF1DX4hpot8sYudB` | 随机播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 10 | Brown Noise (Normal) | `spotify:playlist:37i9dQZF1DX4hpot8sYudB` | 顺序播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 11 | This Is OneRepublic | `spotify:playlist:37i9dQZF1DXbZBKpgxsyXU` | 顺序播放 | SiriKit 意图重放 (`PLAY_EDITORIALPLAYLIST`) |
| 12 | Open Playlist | `spotify:playlist:37i9dQZF1EYkqdzj48dyYq` | 前台跳转 | URL Scheme (`openurl`) |

### 1. 特殊收藏库与常规歌单的区别

- **动作 0（Liked Songs）**：目标 URI 为 `spotify:user:%40:collection`，其反馈类型标识为 `PLAY_MYTRACKS`，实体类型为 `track`。这代表用户个人的「喜欢的歌曲」特殊媒体集合。
- **动作 1～8**：针对具体歌单 ID 的点播，反馈类型均为官方编辑歌单 `PLAY_EDITORIALPLAYLIST`。

### 2. 随机播放与顺序播放的参数对照

动作 9 与动作 10 针对的是完全相同的歌单（`Brown Noise`，ID 为 `37i9dQZF1DX4hpot8sYudB`），构成了天然的对照组：

- **动作 9（开启随机）**：
  ```json
  "player_options_override": {
    "shuffling_context": true
  }
  ```
- **动作 10（常规顺序播放）**：
  ```json
  "player_options_override": {}
  ```
通过该字段的取值差异，可以明确控制播放队列是否启用随机模式。

### 3. 意图重放与 URL Scheme 的混合

动作 12 并不是 SiriKit 意图，而是系统动作：

```text
is.workflow.actions.openurl
```

其参数直接填入了 `spotify:playlist:37i9dQZF1EYkqdzj48dyYq`。运行此动作会强行从前台跳转到 Spotify App 并展示该歌单页面，无法像前 12 个动作一样在后台静默触发。

### 4. 点播指令携带的 `spotify:play-command`

在前 12 个动作的 `IntentData` 中，均包含一段 Base64 编码的播放调度指令。以「Today's Top Hits」为例，解码后的 JSON 结构如下：

```json
{
  "intent": "PLAY",
  "result": "SUCCESS",
  "action": "spotify:nl:CAASEMh9Bx+1FkGNqb5...",
  "context": {
    "uri": "spotify:playlist:37i9dQZF1DXcBWIGoYBM5M",
    "url": "context://spotify:playlist:37i9dQZF1DXcBWIGoYBM5M",
    "metadata": { "autoplay_candidate": "true" }
  },
  "feedback_details": {
    "playlist_name": "Today's Top Hits",
    "entity_type": "playlist",
    "uri": "spotify:playlist:37i9dQZF1DXcBWIGoYBM5M",
    "artwork": [{ "url": "https://i.scdn.co/image/...", "width": 640, "height": 640 }]
  },
  "feedback_id": "PLAY_EDITORIALPLAYLIST",
  "play_origin": { "feature_identifier": "voice-assistant-siri", "referrer_identifier": "voice" },
  "play_options": {
    "always_play_something": true,
    "initially_paused": false,
    "playback_id": "2fa385aaed22443589a107ed1afc4a83",
    "player_options_override": { "shuffling_context": true },
    "session_id": "c87d071f-b516-418d-a9be-7f869cf23b57"
  }
}
```

其中包含两个关键限制：
1. `action` 字段（`spotify:nl:...`）是云端服务在生成该意图时下发的一次性授权签名。
2. 调度数据中绑定了特定的 `playback_id` 与 `session_id`。

如果在文本层面强行修改 `context.uri` 替换为自己的歌单，由于缺少有效匹配的新签名，Spotify 服务端容易判定无效。因此，点播特定内容通常需要设备在 iOS 17 环境下实际语音点播一次以抓取有效意图；若仅需日常恢复播放，直接使用单动作版更为稳定。

> 自动化场景配置

恢复播放快捷指令通常配合系统的「个人自动化」使用：

1. **车载蓝牙连接自动播放**：
   - 打开「快捷指令」App $\to$「自动化」$\to$ 点击右上角「+」新建自动化；
   - 选取「蓝牙」触发器 $\to$ 设备选择汽车车载蓝牙 $\to$ 勾选「立即运行」；
   - 执行操作添加「运行快捷指令」，选取 `Play Media` 即可。
2. **轻点机身背面播放**：
   - 进入系统「设置」$\to$「辅助功能」$\to$「触控」$\to$「轻点背面」；
   - 在「轻点两下」或「轻点三下」中绑定 `Play Media`，实现不亮屏触控恢复播放。

> 参考资料

- Reddit 讨论帖：[Shortcut to play whatever was last being played on Spotify](https://www.reddit.com/r/shortcuts/comments/1r93mgk/shortcut_to_play_whatever_was_last_being_played/)（车载蓝牙自动恢复播放讨论与单动作出处）
- Reddit 原帖：[Spotify Actions in Shortcuts, I'm trying to bring?](https://www.reddit.com/r/shortcuts/comments/1h8p1yn/spotify_actions_in_shortcuts_im_trying_to_bring/)（13 动作点播合集发布帖，作者 Budget_Height3778）
- Reddit 工具帖：[Spotify action generator](https://www.reddit.com/r/shortcuts/comments/1qv2psb/spotify_action_generator/)（通过 Actions App 生成点播动作）
- 数据归档：[12 Spotify Actions JSON 模板](https://pastebin.com/51vcLw4V)
- 官方说明：[Spotify 支持 Siri 语音控制官方文档](https://support.spotify.com/us/article/siri/)
