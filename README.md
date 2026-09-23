# SephiriaF8QB（NPC 废话化）v1.0.0

把游戏里**所有非玩家角色的对话气泡文字**换成菲比啾比风格的胡话。
装上即用，没有快捷键，没有开关界面。

一句台词不再是干巴巴一个词，而是一句随机拼出来的"话"，例如：

```
呜…菲比啾比，啾比。
菲八啾比～
菲比，菲比啾啾比！
```

语气词（呜 / 唔 / 啊 …）**只会出现在句首**，不会挂在句子末尾，所以永远不会有
「…啾比啊。」这种听起来话没说完的结尾。

而 **Boss 开场 / 过场台词的最后一句**固定为 `菲八啾比！`（前面几句照常随机）。

---

## 它会改什么

| 场景 | 是否替换 |
|---|---|
| 城镇 NPC 对话（对话气泡里的台词） | ✅ |
| Boss 台词 / 开场台词 | ✅（**最后一句固定为「菲八啾比！」**） |
| 地牢里怪物、流浪者的喊话 | ✅ |
| 可交互物件（石碑、雕像、牢里的鼹鼠…）的台词 | ✅ |
| Lore / 独白类气泡 | ✅ |
| 过场动画里的 Say 节点 | ✅ |
| **玩家自己的聊天消息** | ❌ 不动（走的是另一套气泡 prefab） |
| NPC 头顶显示的**名字** | 默认不动（可在配置里打开 `ReplaceSpeakerName`） |
| 对话**选项按钮**上的文字 | ❌ 不动（那是玩家选项，不是 NPC 台词） |

替换发生在文字进入气泡的最后一刻，所以打字机动画、气泡大小、
「这句说完了」的判断全都按正常台词处理，不会出现空气泡或气泡卡住。

---

## 安装

1. 本压缩包里的目录结构和游戏目录一一对应，直接**解压到游戏根目录**
   （`D:\steam\steamapps\common\Sephiria\`），遇到同名文件选择覆盖。
   也就是最终会有这两个文件：

   ```
   Sephiria\BepInEx\plugins\SephiriaF8QB.dll
   Sephiria\BepInEx\config\com.sephiria.f8qb.cfg     ← 已随包附带
   ```

   > 如果你之前装过旧名的 **SephiriaJabber**，请先删掉
   > `BepInEx\plugins\SephiriaJabber.dll`。两个同时装会**叠加替换**（一句话被换两次）。
   > 旧的 `BepInEx\config\com.sephiria.jabber.cfg` 已经没人读了，可以一并删掉。

2. 启动游戏。玩家角色出现几秒后，屏幕上方会闪一行

   ```
   菲比啾比！
   ```

   看到这行就说明 mod 已经加载了。然后随便找个 NPC 说话试试。

### 前置条件

- 需要 **BepInEx 6**（Unity Mono 版）。判断方法：看 `BepInEx\core\` 里有没有
  `BepInEx.Core.dll`——有就是 v6，只有 `BepInEx.dll` 就是 v5。
  这个 dll 是给 v6 编的，放到 v5 上会**静默不加载**（日志里一个字都没有）。

---

## 配置

配置文件是 `BepInEx\config\com.sephiria.f8qb.cfg`（**注意是 GUID 名，不叫
SephiriaF8QB.cfg**）。第一次进游戏会自动生成，之后用记事本改。
**改完要重启游戏才生效**（BepInEx 的配置不会热更新）。

### [General]

| 键 | 默认 | 说明 |
|---|---|---|
| `Enabled` | `true` | 总开关。设 `false` 就恢复原文。 |
| `ShowNotice` | `true` | 开局显示那行「菲比啾比！」提示。 |
| `NoticeDelaySeconds` | `6` | 提示延迟多少秒出现。游戏**只有一个系统消息位**，各 mod 的提示都往那里写；默认 6 秒是为了避开 Sephiria Hidden Door 那条「进城提示」（它大约 1.5 秒时出现）。设 `0` = 尽快显示。 |
| `DebugLog` | `false` | 把每条被替换的台词打进日志。排错用。 |

### [Phrases] —— 台词怎么拼

一句台词 = **若干个小句** + **随机语气词** + **结尾标点**：

```
[语气词…] 小句 [，| Separator] 小句 标点
```

> 语气词只会加在**句首**（后面跟一个省略号，如「呜…菲比啾比。」），
> 不会出现在小句末尾或结尾标点之前。

| 键 | 默认 | 说明 |
|---|---|---|
| `PhraseList` | `菲比啾比,菲八啾比,菲比啾啾比,菲比,啾比` | 词池，每个小句从这里随机挑一个。**随便改**，任何文字都行。 |
| `Mode` | `Random` | `Random` = 随机挑；`Cycle` = 按顺序轮；`Fixed` = 永远用 `FixedPhrase`。 |
| `FixedPhrase` | `菲比啾比` | `Mode = Fixed` 时用的那个词。 |
| `MinClauses` | `1` | 一句最少几个小句。 |
| `MaxClauses` | `3` | 一句最多几个小句（在最小/最大之间随机）。 |
| `Repeat` | `0` | 强制小句数量。`0` = 用上面的随机范围。 |
| `Separator` | （空） | 没抽到逗号时小句之间用什么连。空 = 直接连写。 |
| `CommaChance` | `0.5` | 两个小句之间放全角「，」的概率。 |
| `Interjections` | `呜,唔,呃,啊,嗯,哦,呀,啦,欸` | 语气词池，只可能加在**句首**（形如「呜…」）。 |
| `InterjectionChance` | `0.45` | 句首出现语气词的概率。`0` = 完全不要语气词。 |
| `Punctuation` | `。,！,？,～,…,！？` | 结尾标点，**每句必带一个**。 |
| `MatchLength` | `false` | 打开后按原台词长度决定小句数量，长台词还是长的。会盖掉 Min/MaxClauses。 |
| `MaxRepeat` | `8` | `MatchLength` 打开时的小句数量上限。 |
| `ReplaceSpeakerName` | `false` | 连气泡上方的**名字**一起替换（那样就分不清谁在说话了，默认关）。 |

### [Boss] —— Boss 的收尾句

| 键 | 默认 | 说明 |
|---|---|---|
| `BossLastLineEnabled` | `true` | 把 Boss 脚本化台词序列（开场 / 过场）的**最后一句**固定下来。前面的句子照常随机。 |
| `BossLastLine` | `菲八啾比！` | 那一句用什么。 |
| `ScanIntervalSeconds` | `2` | 多久扫一次场上的 Boss AI 去读它的台词表。 |

**它是怎么认出"最后一句"的**：每个 Boss AI 的台词序列都是一个数组
（`UnitAI_QQBoss` 的 `openingSpeeech`、`UnitAI_QBoss` 的 `helloSpeeech` / `bye1Speeech`、
`UnitAI_QBossAdv` 的 `helloSpeech` / `chapter5ClearSpeech`、`UnitAI_QQQBoss` 的 `byeSpeech`…），
而游戏是**从头到尾顺序播**这个数组的：

```csharp
foreach (LocalizedString s in openingSpeeech) {
    CreateSpeechBubbleOpeningRabbit(s.key);
    while (IsSpeaking()) yield return null;
}
```

所以**数组的最后一个元素就是那串台词的收尾句**。mod 每 2 秒把场上 Boss AI 的这些数组
取最后一个元素记下来，气泡原文一匹配上就直接换成 `BossLastLine`。
好处是不需要知道"谁在说话"（气泡在设文字那一刻还不知道自己的 speaker），
而且是在文字写进去**之前**就决定的，屏幕上不会看到文字突然变一下。

---

## 多人 / 联机

这个 mod 只改**你本地画面上**的文字，不写任何游戏状态、不发任何网络消息。

- NPC 气泡本来就是每个客户端各自生成的（服务器只发一句「说这句」，客户端自己建气泡），
  所以**客机装了就生效**，主机装没装都不影响你；主机自己也能看到。
- 你自己的聊天消息不受影响，别人看到的还是你打的字。
- 因此它**不可能导致掉线或不同步**。

---

## 排错

- **完全没反应**：看 `BepInEx\LogOutput.log`，搜 `SephiriaF8QB`。
  - 有 `v1.0.0 loaded` → mod 加载了，往下看。
  - 有 `FAILED to patch` → 游戏更新改了方法签名，把整段报错发我。
  - 一个字都没有 → 大概率是 BepInEx 版本不对（见上面「前置条件」）。
- **Boss 收尾句没变成固定的**：把 `DebugLog` 设成 `true` 重启，日志里会打印
  `boss closing line: "..."`，每行一条。一行都没有 = 那个 Boss 的台词不是用
  `LocalizedString[]` 数组存的（告诉我是哪个 Boss，我单独补）。
- **改了配置没用**：确认改的是 `com.sephiria.f8qb.cfg`，并且**重启过游戏**。
- **想完全恢复**：把 `Enabled` 设成 `false`，或者删掉
  `BepInEx\plugins\SephiriaF8QB.dll`。

---

## SephiriaF8QB v1.0.0 (English, short)

Replaces every non-player speech bubble line (town NPCs, bosses, monsters, props, lore,
cutscenes) with nonsense built from a word pool, random interjections and end punctuation,
e.g. `呜…菲比啾比，啾比。` / `菲八啾比～`. Interjections lead a line only, never trail it.
The closing line of a boss's scripted sequence
(opening / cutscene) is fixed to `菲八啾比！` while the lines before it stay random.

Install: unzip into the game root so the file lands in
`Sephiria\BepInEx\plugins\SephiriaF8QB.dll`. Requires **BepInEx 6**
(`BepInEx\core\BepInEx.Core.dll` exists). A `菲比啾比！` notice appears a few seconds after
the player spawns, which confirms it loaded.

Config: `BepInEx\config\com.sephiria.f8qb.cfg` (GUID-named, not `SephiriaF8QB.cfg`),
generated on first launch, restart the game after editing.
`[Phrases]` shapes a line: `PhraseList` (word pool), `MinClauses` / `MaxClauses`,
`Separator`, `CommaChance`, `Interjections`, `InterjectionChance`, `Punctuation`.
`[Boss]` controls the fixed closing line: `BossLastLineEnabled`, `BossLastLine`.

Boss closing lines are detected from the boss AI's `LocalizedString[]` speech arrays, which
the game plays front to back, so the last array element IS the last line of the sequence.

Your own chat messages and conversation choice buttons are NOT affected. Multiplayer safe:
it only rewrites text locally and never touches game state or the network, so it works on a
client whether or not the host has it.
