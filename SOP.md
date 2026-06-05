# SOP — 如何使用 style-mirror

这是一份操作手册，讲清楚你日常怎么用这套系统让 AI 越来越「像你」地润色英文。所有操作都是**复制粘贴模板 + 和 agent 对话**，没有 app、没有数据库、不用写代码。

> 一句话原理：系统真正学习的，不是你的原文，也不是你的最终版，而是**「agent 的润色版 → 你的最终版」之间的差异**。那个 diff 精确地暴露了你和「标准/AI 英语」的区别，那就是你的风格。所以——**你每改一次 agent 的版本，就是在教它一次。**

---

## 0. 准备：在哪里操作

在这个文件夹（`/opt/processes/style-mirror/`）里开一个 agent 会话即可。agent 每次会话开头会自动读 [AGENTS.md](AGENTS.md)，所以你**不需要**每次重新解释系统，直接用下面的模板就行。

模板都在 [contexts/memory/PROMPTS.md](contexts/memory/PROMPTS.md)，需要时复制粘贴。

---

## 1. 第一次使用：ONBOARD（只做一次）

**目的**：系统第一天是空的，先用你的真实写作喂一个「弱底子」，否则 agent 只能瞎猜。

**步骤**：

1. 找 3–8 段**你自己真实写过、且当时满意**的英文（不要用 AI 写的）。尽量覆盖你常用的场景：比如 1–2 封给老板的邮件、几条同事群聊、一段文档。
2. 复制 PROMPTS.md 里的 **ONBOARD** 模板，把每段样本贴在对应的 `[scenario: ...]` 标签下面，发给 agent。
3. agent 会做三件事：
   - 把样本原样存进 `samples/`
   - 填好 [rules/USER.md](rules/USER.md)（你的母语、英语主要用在哪、你对现在 AI 润色的不满）
   - 保守地提炼出一些风格规则，写进档案——但**全部标成 `seed`（弱先验）**

**重要心态**：`seed` 规则 agent 不会静默套用，只会当「建议」并标注出来。真正的学习要等你后面用编辑去确认。所以 ONBOARD 只是让它别从零开始，不是终点。

---

## 2. 日常主循环：POLISH → 编辑 → LEARN

这是你每天会重复的三步。**核心纪律：POLISH 和 LEARN 要在同一个会话里**，这样 agent 还记得它刚才给你的润色版。

### 第 1 步：POLISH（让它润色）

复制 **POLISH** 模板，贴上你的原文：

```
POLISH [scenario: email_to_boss]

<你的原始英文>
```

- 场景标签可选；不写的话 agent 会自己推断并告诉你。
- agent 会先修语法，再套用它**已经学会**的你的风格：
  - `established`（已确立的）规则 → 静默套用
  - `tentative`（试探性的）规则 → 套用但会在 `Notes` 里标出来说「这是猜的，不对就改」
  - `seed` / `contested`（有争议的）→ 不强加，保留你原来的措辞
- 润色之外，agent 还可能给一个**可选的「💡 更地道的说法」建议块**（1–3 条「你的写法 → 更地道的说法 + 为什么」），供你学习更好的表达。
  - **默认润色仍然忠于你的原话**，这些建议不会塞进润色版里，也不会写进任何档案——用不用、要不要把哪条改进你的最终版，**完全由你决定**。
- **POLISH 阶段 agent 不写任何文件**，纯产出。

### 第 2 步：编辑（你来改）⭐ 最关键

把 agent 给的润色版，**改成你真正会拿去发的样子**。

这一步是整个系统的命根子。改得越诚实、越接近「你真实会怎么说」，系统学得越准。尤其是：
- agent 标了「试探性猜测」而你觉得不对 → **大胆改回去**
- agent 把你原来的词换掉了、但你更喜欢原来的 → **改回原来的**（这叫「回退」，是最强的学习信号，说明 agent 过度纠正、跑偏了）

### 第 3 步：LEARN（把最终版发回，让它学）

复制 **LEARN** 模板，贴上你刚改好的最终版：

```
LEARN. Here is the final version I actually used. ...

<你的最终英文>
```

agent 会：
- 对比「它的润色版 → 你的最终版」，逐处判断是 拼写 / 它的错误 / 风格信号 / 回退
- 把风格信号提炼成规则，更新置信度（详见下面第 4 节）
- 在 `contexts/sessions/` 写一份原始证据文件（你的原文、它的版本、你的最终版都逐字存着，可回溯）
- 用一两句话告诉你：**这次学到了什么、到什么置信度了**

---

## 3. 一个完整例子

```
你 →  POLISH [scenario: email_to_boss]
      I would like to kindly ask you to utilize the new dashboard.

agent → Polished:
        > I'd like to ask you to use the new dashboard.
        Notes:
        - (tentative) 把 "utilize" 换成 "use"——根据你样本猜的，不对就改

你（编辑成真实版）→  I want you to use the new dashboard.

你 →  LEARN.
      I want you to use the new dashboard.

agent → 学到了：
        - 你偏好 "want to" 胜过 "would like to"（lx_would_like，第 1 次，tentative）
        - "utilize → use" 被你确认了（lx_utilize，转 tentative）
        - 你删掉了 "kindly"（lx_kindly，tentative）
        已记录为 s_20260603_001。
```

下次你再写类似的邮件，这些规则攒够次数后，agent 就会**自动**帮你换好，不用再标注。

---

## 4. 系统怎么「记住」你（你只需理解，不用操作）

每条风格规则都有一个**置信度**，决定 agent 敢不敢自动用它：

| 置信度 | 含义 | agent 怎么用 |
|--------|------|--------------|
| `seed` | 只来自 onboarding 样本 | 不自动用，最多当建议 |
| `tentative` | 真实编辑里见过 1 次 | 会用，但会标注「猜的」 |
| `established` | 见过 ≥3 次、跨 ≥2 个不同会话 | **静默自动用** |
| `contested` | 既有支持也有反例 | 不用，回退到你的原话 |

**关键规则：单次编辑永远升不到 `established`。** 必须反复出现才会被「静默套用」；一旦出现反例立刻降级。**升级慢、降级快**——这样 agent 不会因为你某一次的临时心情就跑偏。

---

## 5. 每周保养：DISTILL（建议每周或每攒 ~10 次循环做一次）

复制 **DISTILL** 模板发给 agent。它会：
- 重新核算所有规则的置信度，把稳定的升级成 `established`
- 处理有争议（`contested`）的规则，必要时让你拍板
- 清理那些只出现一次、很久没复现的「噪音」规则
- 检查 `contexts/sessions/INDEX.md` 里的 `diff_size`（每次你改动的量）**是不是在变小**——变小就说明 agent 越来越像你了

---

## 6. 怎么判断系统真的在进步

1. **置信度在爬升**：同类表达写几次后，某条规则从 `tentative` 升到 `established`；当 agent 第一次「悄悄就帮你改对了、还不用标注」时，闭环就成了。
2. **你改得越来越少**：`sessions/INDEX.md` 里的 `diff_size` 趋势向下，就是它在向你的声音收敛。
3. **可回溯**：随便挑一条 `established` 规则，顺着它链接的 session 文件翻回去，应该能看到 ≥3 次真实编辑撑着它——不是凭空捏的。

---

## 7. 几条用好它的小建议

- **诚实地改**：按你真实会说的方式改，不要迁就 agent 的版本，否则它学到的是「将就」而不是「你」。
- **错了就纠**：agent 标了猜测而你觉得不对，一定要在 LEARN 里改掉——纠正（尤其是回退到你原话）是它能拿到的最强信号。
- **POLISH 和 LEARN 配对、同会话**：换了新会话的话，记得把上一次的润色版也一起贴回去。
- **样本不嫌多**：随时可以再补几段真实写作进 `samples/`，让底子更厚。
- **别指望第一周就完美**：前 ~5–10 次循环它还在摸你的风格，标注的都是猜测。这正是它该有的样子——你的纠正就是燃料。

---

## 速查

| 我想… | 用哪个模板 | 在哪 |
|-------|-----------|------|
| 第一次设置 | ONBOARD | [PROMPTS.md](contexts/memory/PROMPTS.md) |
| 润色一段文字 | POLISH | 同上 |
| 教它我的编辑 | LEARN | 同上 |
| 每周保养 | DISTILL | 同上 |
| 看它学到了什么 | 直接读 | [rules/style/STYLE_PROFILE.md](rules/style/STYLE_PROFILE.md) · [LEXICON.md](rules/style/LEXICON.md) |
| 看历史记录 | 直接读 | [contexts/sessions/INDEX.md](contexts/sessions/INDEX.md) |
