# 后期成片合成与两遍式字幕压制规范（Post-Production Spec）

## 硬切与响度
- 5 段视频按 `trim_start=0`，`trim_duration` 依次为 `6.0 / 6.0 / 6.0 / 7.0 / 5.0`（`cut_mode=exact, volume=1.0`）直接硬切拼接（Direct picture cuts，严禁添加渐变/擦除等转场特效），开启 `audio_loudnorm=true`，输出分辨率 `1112×834`（4:3）。

## 第一遍合成（底部居中 · 中英双语同屏对白硬字幕）
- 样式参数：`mode="file"`, `font="source_han_sans_cn"`（思源黑体）, `size="small"`, `outline=true`, `position="bottom"`。
- **防拆分铁律**：在 `1112×834` 画幅下，单行安全宽度为 `932px`（字号 42px）。必须将全片 5 句台词按自然语流切分为 **14–17 条短句双语 SRT 字幕**，每条字幕严格满足：**第 1 行中文 ≤ 14 个汉字**、**第 2 行英文 ≤ 32 个半角字符**（单行估算宽度 ≤ 750px）。合成后必须检查返回元数据满足 `source_cue_count == rendered_cue_count` 且 `max_lines == 2`，确保每一条字幕都是“上行中文 + 下行英文”双行同屏稳定显示，绝不触发自动折行拆分成先后交替闪烁的单语字幕；观众笑声区间不排字幕。

## 第二遍合成（顶部居中 · 开场 2.8 秒剧集名片头介绍）
- 以第一遍已压制底部双语字幕的 30.0 秒成片作为唯一视频源（`trim_start=0, trim_duration=30.0, cut_mode=exact, volume=1.0`）。
- 样式参数：`mode="file"`, `font="source_han_sans_cn"`, `size="small"`, `outline=true`, `position="top"`。
- 时间轴与格式：仅设置 1 条 SRT 字幕，时间固定为 `00:00:00,150 --> 00:00:02,800`（开场显示 2.65 秒后消失）：
  - 第 1 行：`EpXX《中文剧集名（核心副标题）》`（总宽控制在 ≤ 900px，即汉字 ≤ 18 字）
  - 第 2 行：`(English Episode Title)`（英文 ≤ 34 字符）
