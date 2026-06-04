# LLL 触发最佳实践
## Calibrating effort and thinking depth 调整 effort 和思考深度
不同的参数在解决任务时有不同的表现，应根据任务复杂性以及需求结合 token 消耗选择合适的参数。
- `max`
  - 在某些使用场景下，`max` 确实能够提升性能。但随着代币使用量的增加，其带来的收益可能会逐渐下降。
  - 此外，这种设置有时也容易过度思考。建议在那些需要高度智能处理的任务中测试 `max` 模式。
- `xhigh`
  - 在大多数编程和自动化任务中，`xhigh` 才是最佳选择。
- `high`
  - 此设置有助于在令牌使用效率与智能水平之间取得平衡。对于那些对智能水平要求较高的应用场景，建议至少投入 `high`。
- `medium`
  - 对于那些在追求降低 `token` 使用成本的同时，又不愿牺牲智能水平的场景来说，这是个不错的选择。
- `low`
  - 用于处理那些时间要求紧迫、具有明确时间限制的任务，以及那些对延迟敏感但并不需要高度智能处理的工作负载。 


如果在处理问题时发现回答过于肤浅，要么提升参数设置，如果受限于 `token` 使用成本，可以添加以下提示词。
```text
This task involves multi-step reasoning. Think carefully through the problem before responding.

该任务涉及多步推理。在作答前请仔细思考问题。
```

模型有时候会过度思考，占用过多的时间和 `token`，这时候可以通过以下提示词来引导模型更快地给出答案。
```text
Thinking adds latency and should only be used when it will meaningfully improve answer quality — typically for problems that require multi-step reasoning. When in doubt, respond directly.

思考会增加延迟，仅应在能显著提升回答质量时使用——通常适用于需要多步推理的问题。如有疑问，请直接回复。
```

与以上相反，如果觉得模型某个参数回答不够深入，应该先考虑调整参数设置，如果需要更精细的控制，则可以通过提示词做进一步优化。

---

## Tool use triggering  工具使用的触发条件
- 当 `effort` 设置为 `high`、`xhigh` 或更高时，模型在搜索和编码任务中会倾向于使用工具来解决问题。
- 如果希望 LLM 更多的使用工具来解决问题，可以在提示词中增加提示用于告知模型什么时候使用工具，或者通过参数强制让 LLM 使用工具。

---

## Tone and writing style  语气与写作风格
对于某些需要特定语气的产品，可以通过提示词来引导
```text
例如，如果需要产品语音更亲切、更像日常对话，可以添加以下内容：

Use a warm, collaborative tone. Acknowledge the user's framing before answering.
使用温暖、协作的语气，在回答前先认可用户的表达方式。
```

---

## Controlling subagent spawning   控制子代理的生成过程
子代理可以通过提示词来调整，比如告知在什么情况下明确生成子代理
```text
以下是一个编程场景。

Do not spawn a subagent for work you can complete directly in a single response (e.g. refactoring a function you can already see).
Spawn multiple subagents in the same turn when fanning out across items or reading multiple files.

不要为可以直接在单个响应中完成的工作创建子代理（例如，重构一个你已经能看到的函数）。
在同一次行动中，当向多个物品扩散或读取多个文件时，生成多个子代理。

```

---

## Design and frontend defaults    设计与前端默认设置
设计和前端默认设置一般会受到模型的默认参数设置的影响。
为了适配在不同场景【金融、电商、医疗】下的 UI 需求，有两种解决方式。 

1.明确提出具体的替代方案。该模型会严格遵循既定的规范来运作。
```text
为名为AEFRM的保健品品牌设计一个桌面版首页。
视觉方向应来自一种冷色调的单色氛围，采用浅银灰色调，逐渐加深为蓝灰色和近乎黑色，类似于雾化的金属表面。
页面应感觉清晰而有条理，具有强烈的结构感和克制感。
在整个页面中使用这种色调系统，而不是引入鲜艳的强调色。
使用上传的黑白英雄设计图片。
布局应采用清晰的水平分区，并使用居中设置的最大宽度容器。卡片、按钮、输入框和媒体框架的边角半径应统一为4px。外边距应足够宽松，每个区域周围留有充足空白空间，使页面呼吸感更佳。
字体应采用方形、棱角分明的无衬线体，字母间距比通常更大，尤其在标题和导航处，使文字看起来更显规整，避免显得拥挤。标题文字可使用大号且全大写的格式，而正文则保持简短紧凑。副标题应使用 Alumni Sans SC 字体，字号为 4-6 像素，如同位于底部中央角落的小字般精致。
对于结构，首先从一个英雄段落开始，包含一句强有力的产品陈述、一段简短的支撑性文字，以及一个干净的产品占位图或包装展示框。在其下方，添加一个包含三到四个区块的益处网格，然后是配方或成分部分，最后是行动号召（CTA）。
按钮应保持扁平且精准，使用过渡效果实现轻微的悬停变化：所有元素在160毫秒内平滑淡出，亮度和边框对比度略有调整，避免使用剧烈的动画效果。
配色方案应保持在以下范围内：  
#E9ECEC, #C9D2D4, #8C9A9E, #44545B, #11171B。
```

2.在构建模型之前，先让模型提出各种可能的方案。这样做可以打破默认设置，让用户能够自主掌控设计过程。如果你之前一直依赖 temperature 来获得不同的设计效果，不妨试试这种方法；它能让每次运行产生的结果都有所不同。
```text
Before building, propose 4 distinct visual directions tailored to this brief (each as: bg hex / accent hex / typeface — one-line rationale). Ask the user to pick one, then implement only that direction.

在开始设计前，根据本简报提出4种不同的视觉方向（每种包括：背景色/强调色/字体——用一句话说明理由）。请用户从中选择一种，并仅采用该方向进行实现。
```

---

## Interactive coding products   交互式编程产品
交互式编程虽然在长时间交互上能够保证代码质量，但是也增加了很多 `token` 成本和时间成本，还有一个问题是长上下文交互的注意力下降问题。
- 为了在以上问题中取得平衡，建议在参数设置上优先 `high` 或者 `xhigh`。
- 在限制交互次数或者 `token` 使用时，第一次的交互很重要，需要明确说明任务内容、意图以及相关的限制条件。
- 第一次交互越明确，后续的交互次数越少的情况下代码质量也不会降低，且能保证 `token` 的消耗和时间成本的降低。

---

## Code review harnesses  代码审查工具/手段
在进行代码审查时，需要新开窗口或者使用不同的 LLM 进行审查，以避免上下文的干扰和注意力下降问题。
可以结合以下提示词
```text
Report every issue you find, including ones you are uncertain about or consider low-severity. Do not filter for importance or confidence at this stage - a separate verification step will do that. Your goal here is coverage: it is better to surface a finding that later gets filtered out than to silently drop a real bug. For each finding, include your confidence level and an estimated severity so a downstream filter can rank them.

报告你发现的每一个问题，包括那些你不确定或认为严重程度较低的问题。现阶段不要根据重要性或置信度进行筛选——这将在后续的验证步骤中完成。你的目标是覆盖范围：最好暴露一个后来会被过滤掉的问题，而不是悄然遗漏一个真实漏洞。对于每个发现，请注明你的置信程度和估计的严重程度，以便下游筛选环节进行排序。
```

如果希望一次性审查完成，那就不要设置模糊的边界，尽量明确审查范围：
```text
report any bugs that could cause incorrect behavior, a test failure, or a misleading result; only omit nits like pure style or naming preferences.

请报告所有可能导致错误行为、测试失败或产生误导性结果的漏洞；只需忽略那些纯粹与风格或命名方式相关的问题即可。
```

---

# General principles  一般原则
## Be clear and direct  要清晰直接。
- 如果能够明确说明自己期望得到的结果，就能提升效果。如果你希望得到“超出预期”的表现，那就应明确提出要求，而不是指望模型能从含糊的指令中自行理解。
- 可以把 LLM 当做聪明但缺乏经验的员工，它不了解工作规范和流程，所以，越能清晰地表达自己的需求，得到的结果就会越好。
- 黄金法则：向同事展示任务要求时，尽量少提供背景信息，然后请他们按照这些要求来执行。如果同事都搞不清楚，那 LLM 也肯定搞不懂。
  - 请明确说明所需的输出格式和各种限制条件。
  - 当步骤的顺序或完整性很重要时，请使用编号列表或项目符号来按顺序列出操作指南。
对比示例：
```text
效果更差：
Create an analytics dashboard
创建一个分析仪表盘

更有效：
Create an analytics dashboard. Include as many relevant features and interactions as possible. Go beyond the basics to create a fully-featured implementation.
创建一个分析仪表盘。尽可能包含更多相关功能和交互操作，超越基础功能，打造一个功能齐全的实现方案。
```

---

## Add context to improve performance   添加相关背景信息以提高性能
在给出指令时，如果能说明其中的背景或原因，比如向 LLM 解释为什么某种行为很重要，那么就能帮助 LLM 更好地理解你的意图，从而给出更恰当的回应。
对比示例：
```text
效果更差：
NEVER use ellipses
永远不要使用省略号

更有效：
Your response will be read aloud by a text-to-speech engine, so never use ellipses since the text-to-speech engine will not know how to pronounce them.
您的回复将由文本转语音引擎朗读，因此请勿使用省略号，因为文本转语音引擎无法正确发音。
```

---

## Use examples effectively  有效运用例子进行说明
要几个设计得当的例子，就能显著提升 LLM 的准确性和一致性。这类方法被称为“少样本提示”或“多样本提示”。

在添加示例时，请确保这些示例：
- 相关提示：请确保模拟的情况尽可能贴近实际使用场景。
- 多样性：能够涵盖各种边缘情况，且变化足够大，从而避免 LLM 识别出那些非预期的模式。
- 结构化处理：将各个示例用 <example> 标签括起来（多个示例则用 <examples> 标签括起来），这样 LLM 就能将它们与指令区分开来。
- 为了获得最佳效果，请提供 3 到 5 个示例。

---

## Structure prompts with XML tags    使用 XML 标签来构建提示信息的结构
XML 标签有助于 LLM 准确解析复杂的指令内容，尤其是当指令中同时包含各种指示、背景信息、示例以及可变参数时。

将每种类型的内容用相应的标签括起来（例如 `<instructions>` 、 `<context>` 、 `<input>` ），可以有效避免误解。

最佳实践：
- 在所有的提示语中，请使用一致且具有描述性的标签名称。
- 当内容具有自然的层次结构时，可以使用嵌套标签来标记：文档位于 `<documents>` 内部，而 `<documents>` 又位于 `<document index="n">` 内部。

---

## Give LLM a role  给大模型一个角色吧。
在系统提示中设定相应的角色，有助于让 LLM 的行为和语气更符合你的使用需求。哪怕只修改一句话，也会产生效果：

```text
system: You are a helpful coding assistant specializing in Python.
系统：你是一位擅长 Python 的有帮助的编程助手。
```

---

## Long context prompting  长上下文提示/长文本背景信息提示
在处理大型文档或数据量庞大的输入内容时（超过 20,000 个标记），请仔细设计提示词，以获得最佳效果：
- 将长篇内容放在顶部：请把冗长的文档或输入内容放在提示语的顶端，也就是在查询内容、指令和示例的上方。这样做能显著提升所有模型的性能。
- 使用 XML 标签来组织文档内容和元数据：当处理多个文档时，应使用 `<document>` 标签将每个文档包裹起来，并再使用 `<document_content>` 、 `<source>` 等子标签来标注各种元数据，从而提高文档的清晰度。
- 多文档结构示例：
```xml
<documents>
  <document index="1">
    <source>annual_report_2023.pdf</source>
    <document_content>
      {{ANNUAL_REPORT}}
    </document_content>
  </document>
  <document index="2">
    <source>competitor_analysis_q2.xlsx</source>
    <document_content>
      {{COMPETITOR_ANALYSIS}}
    </document_content>
  </document>
</documents>

Analyze the annual report and competitor analysis. Identify strategic advantages and recommend Q3 focus areas.
分析年度报告和竞争对手分析，识别战略优势，并提出第三季度的重点工作方向。
```
- 用引号标注关键内容：在处理长篇文档时，先让 LLM 引用文档中的相关部分，然后再执行任务。这样有助于 LLM 忽略文档中的其他无关内容，从而更准确地完成任务。
```xml
You are an AI physician's assistant. Your task is to help doctors diagnose possible patient illnesses.
你是一位AI医生助手，任务是帮助医生诊断患者可能的疾病。

<documents>
  <document index="1">
    <source>patient_symptoms.txt</source>
    <document_content>
      {{PATIENT_SYMPTOMS}}
    </document_content>
  </document>
  <document index="2">
    <source>patient_records.txt</source>
    <document_content>
      {{PATIENT_RECORDS}}
    </document_content>
  </document>
  <document index="3">
    <source>patient01_appt_history.txt</source>
    <document_content>
      {{PATIENT01_APPOINTMENT_HISTORY}}
    </document_content>
  </document>
</documents>

Find quotes from the patient records and appointment history that are relevant to diagnosing the patient's reported symptoms. Place these in <quotes> tags. Then, based on these quotes, list all information that would help the doctor diagnose the patient's symptoms. Place your diagnostic information in <info> tags.
从患者病历和就诊记录中提取与诊断患者所报告症状相关的引述内容，并用 <quotes> 标签将其包含。然后，根据这些引述，列出所有有助于医生诊断患者症状的信息，并用 <info> 标签将诊断信息标注出来。
```

---

## Model self-knowledge  自我认知模型
如果您希望 LLM 能在您的应用程序中正确识别自身，或使用特定的 API 接口，可以使用以下提示词：
```text
The assistant is Claude, created by Anthropic. The current model is Claude Opus 4.8.
```

---

# Output and formatting  输出与格式设置
## Control the format of responses    控制回复的格式
有几种特别有效的方法可以用来控制输出格式：
- 告诉克劳德该做什么，而不是不该做什么。
  - 而不是：“请在回复中不要使用 Markdown 格式”
  - 试试这样表达：“你的回答应该由流畅连贯的段落组成。”
- 使用 XML 格式的标记/指示符
  - 试试这样写：“请将回复中的散文部分用 `<smoothly_flowing_prose_paragraphs>` 标签括起来。”
- 让提示语的风格与期望的输出结果相一致。
  - 您在提示语中使用的格式风格可能会影响 LLM 的响应方式。如果您仍然觉得输出的格式难以控制，请尽量让提示语的格式与您期望的输出格式保持一致。例如，如果去掉提示语中的 Markdown 格式，那么输出内容中的 Markdown 元素也会相应减少。
- 请使用详细的说明来指定具体的格式要求。
  - 为了更好地控制 Markdown 格式和排版方式，请给出明确的指导说明：
 ```text
<avoid_excessive_markdown_and_bullet_points>
When writing reports, documents, technical explanations, analyses, or any long-form content, write in clear, flowing prose using complete paragraphs and sentences. Use standard paragraph breaks for organization and reserve markdown primarily for `inline code`, code blocks (```...```), and simple headings (###, and ###). Avoid using **bold** and *italics*.

DO NOT use ordered lists (1. ...) or unordered lists (*) unless : a) you're presenting truly discrete items where a list format is the best option, or b) the user explicitly requests a list or ranking

Instead of listing items with bullets or numbers, incorporate them naturally into sentences. This guidance applies especially to technical writing. Using prose instead of excessive formatting will improve user satisfaction. NEVER output a series of overly short bullet points.

Your goal is readable, flowing text that guides the reader naturally through ideas rather than fragmenting information into isolated points.
</avoid_excessive_markdown_and_bullet_points>

撰写报告、文档、技术说明、分析或任何长篇内容时，应使用清晰流畅的段落和句子，采用完整的段落分隔进行组织。
使用标准的段落断行来结构化内容，而将 Markdown 主要用于内联代码、代码块（```...```）以及简单的标题（### 和 ###）。
避免使用 **加粗** 和 *斜体*。
除非：a) 您要呈现真正离散的项目，且列表格式是最佳选择，或 b) 用户明确要求使用列表或排序，否则请勿使用有序列表（1. ...）或无序列表（*）。
不要使用项目符号或编号来列出项目，而是将它们自然地融入句子中。
这一建议尤其适用于技术写作。使用纯文字而非过多的格式化，有助于提升用户的满意度。切勿输出一系列过于简短的项目符号。
你的目标是让文字清晰流畅，引导读者自然地理解思路，而不是将信息碎片化为孤立的要点。
```

---

# Tool use  工具的使用
## Tool usage  工具的使用情况
通过提示词控制 LLM 使用工具的频率和时机

为了让 LLM 在默认情况下更主动地采取行动，你可以在系统提示语中加入以下内容：
```text
<default_to_action>
By default, implement changes rather than only suggesting them. If the user's intent is unclear, infer the most useful likely action and proceed, using tools to discover any missing details instead of guessing. Try to infer the user's intent about whether a tool call (e.g., file edit or read) is intended or not, and act accordingly.

<default_to_action>
默认情况下，应实施更改而非仅提供建议。如果用户的意图不明确，应推断出最可能有用的行动并继续执行，通过工具来发现缺失的细节，而不是猜测。尽量推断用户是否意图调用某个工具（例如文件编辑或读取），并据此采取相应行动。
</default_to_action>
```

如果你希望该模型在默认情况下更加谨慎行事，不会轻易采取行动，而只有在被明确要求时才采取行动，那么你可以通过如下提示来引导其行为：
```text
<do_not_act_before_instructions>
Do not jump into implementation or change files unless clearly instructed to make changes. When the user's intent is ambiguous, default to providing information, doing research, and providing recommendations rather than taking action. Only proceed with edits, modifications, or implementations when the user explicitly requests them.
</do_not_act_before_instructions>

<do_not_act_before_instructions>
除非明确指示，否则不要立即实施或修改文件。当用户意图不明确时，应默认提供信息、进行调研并给出建议，而非采取行动。只有在用户明确要求时，才应进行编辑、修改或实施。
</do_not_act_before_instructions>
```

---

## Optimize parallel tool calling   优化并行工具的调用方式
通过提示词优化 LLM 在需要调用多个工具时的行为方式，以提高效率和性能。
```text
<use_parallel_tool_calls>
If you intend to call multiple tools and there are no dependencies between the tool calls, make all of the independent tool calls in parallel. Prioritize calling tools simultaneously whenever the actions can be done in parallel rather than sequentially. For example, when reading 3 files, run 3 tool calls in parallel to read all 3 files into context at the same time. Maximize use of parallel tool calls where possible to increase speed and efficiency. However, if some tool calls depend on previous calls to inform dependent values like the parameters, do NOT call these tools in parallel and instead call them sequentially. Never use placeholders or guess missing parameters in tool calls.
</use_parallel_tool_calls>

<use_parallel_tool_calls>
如果打算调用多个工具且这些调用之间没有依赖关系，请将所有独立的工具调用并行执行。
当操作可以并行完成时，应优先同时调用工具，而不是顺序执行。
例如，在读取3个文件时，可并行运行3次工具调用，以同时将全部3个文件加载到上下文中。
尽可能充分利用并行工具调用，以提高速度和效率。
但若某些工具调用依赖于先前调用的结果来确定参数等依赖值，则不应并行调用，而应按顺序调用。
切勿在工具调用中使用占位符或猜测缺失的参数。
</use_parallel_tool_calls>
```

减少并行执行的提示语
```text
Execute operations sequentially with brief pauses between each step to ensure stability.

按顺序执行操作，并在每一步之间稍作暂停，以确保稳定性。
```

---

# Thinking and reasoning  思考与推理
## Overthinking and excessive thoroughness    过度思考和过于追求细致入微
- 用更具体的指导来取代那些笼统的默认设置。不要简单地说“默认使用 [tool]”，而应给出诸如“当使用 [tool] 能帮助你更好地理解问题时，再使用它”之类的指导。
- 消除过度提示的问题。在之前的模型中未能被正确触发的工具，现在应该能够被正确地触发。像“如果有疑问，请使用 [tool] ”这样的指令，反而会导致过度触发。
- 把 `effort` 作为最后的手段。如果 LLM 仍然表现得过于激进，那就将 `effort` 的数值调低一些。

- 除了设置 `effort` 参数之外，还可以通过提示词来引导模型更快地给出答案，避免过度思考：
```text
When you're deciding how to approach a problem, choose an approach and commit to it. Avoid revisiting decisions unless you encounter new information that directly contradicts your reasoning. If you're weighing two approaches, pick one and see it through. You can always course-correct later if the chosen approach fails.

当你决定如何应对一个问题时，应选择一种方法并坚持下去。除非遇到与你的判断直接相矛盾的新信息，否则不要反复更改决定。如果你在权衡两种方法，就选其中一种并坚持到底。如果所选的方法失败了，你随时可以调整方向。
```
- 如果还是不满意，可以通过 `max_token` 继续限制

---

## Leverage thinking & interleaved thinking capabilities    利用思维与交叉思维的能力
### thinking  思考模式
适当引导 LLM 思考方式
```text
After receiving tool results, carefully reflect on their quality and determine optimal next steps before proceeding. Use your thinking to plan and iterate based on this new information, and then take the best next action.

在收到工具结果后，仔细评估其质量，并在继续之前确定最佳下一步行动。利用你的思考来根据这些新信息进行规划和迭代，然后采取最优的后续措施。
```
思考频率过高时，通过相应提示词控制
```text
Extended thinking adds latency and should only be used when it will meaningfully improve answer quality - typically for problems that require multi-step reasoning. When in doubt, respond directly.

扩展思考会增加延迟，仅应在能显著提升回答质量时使用——通常适用于需要多步推理的问题。如有疑问，请直接作答。
```
### none thinking  非思考模式
当不需要思考时，可以通过以下提示词来引导模型
- 比起具体的操作步骤，我更喜欢一般的指导性建议。比如“仔细思考”这样的提示，往往能带来比详细的书面步骤更出色的推理结果。
- 多轮思考的示例有助于提升思维能力。在少轮思考的示例中使用 <thinking> 标签，向 LLM 展示其推理模式。这样，LLM 就能将这种推理方式应用到更复杂的思考过程中。
- 作为备用方案，可以使用手动推理方式。当思维出现混乱时，可以要求 LLM 逐步分析问题，从而帮助人们进行有条理的推理。可以使用 `<thinking>` 、 `<answer>` 等标记来将推理过程与最终结果清晰地分开。
- 让 LLM 自行检查。可以加上这样的提示：“在完成之前，请先根据[测试标准]来核对你的答案。”这种方法能有效地发现错误，尤其是在编码和数学领域。

---

# Agentic systems  代理系统
## Long-horizon reasoning and state tracking    长期视角的推理与状态跟踪
优秀的模型能够在长时间的交互中保持对话的连贯性和上下文的理解，这对于需要长期推理和状态跟踪的任务尤为重要。

---

## Context awareness and multi-window workflows   上下文感知与多窗口工作流程
### 管理上下文限制
如果当前系统允许压缩上下文保存到外部文件，务必在提示词中加入这些信息，这样 LLM 才能做出相应的处理。否则，当上下文量接近上限时，LLM 可能会试图自行结束当前任务。
```text
Your context window will be automatically compacted as it approaches its limit, allowing you to continue working indefinitely from where you left off. Therefore, do not stop tasks early due to token budget concerns. As you approach your token budget limit, save your current progress and state to memory before the context window refreshes. Always be as persistent and autonomous as possible and complete tasks fully, even if the end of your budget is approaching. Never artificially stop any task early regardless of the context remaining.

当上下文窗口接近容量限制时，系统将自动进行压缩，使您能够从上次中断的地方无限期继续工作。
因此，请不要因令牌预算问题而提前终止任务。
当您接近令牌预算上限时，请在上下文窗口刷新前，将当前进度和状态保存至内存中。
请尽可能保持持续性和自主性，即使预算即将耗尽，也务必完成任务。无论剩余多少上下文，都切勿人为提前终止任何任务。
```

---

## Multi-context window workflows   多上下文窗口工作流
对于需要跨越多个上下文窗口的任务：
- 对于第一个上下文窗口，请使用不同的提示语：利用第一个上下文窗口来建立框架结构（编写测试用例、创建设置脚本），之后再利用后续的上下文窗口来逐步完善待办事项列表。
- 让模型以结构化的方式编写测试用例：在开始工作之前，让 LLM 先创建测试用例，并以结构化的方式对它们进行管理（例如，使用 `tests.json` 之类的标记）。这样有助于提升长期的迭代能力。同时，要提醒 LLM 测试用例的重要性：“绝不能删除或修改测试用例，因为那样可能会导致某些功能缺失或出现错误。”
- 设置提升生命周期质量的工具：鼓励 LLM 编写相应的脚本（例如 `init.sh` ），以便能够顺利地启动服务器、运行测试用例及代码检查工具。这样一来，当从新的环境窗口开始工作时，就可以避免重复做同样的事情。
- 从头开始 vs 合并数据：当上下文窗口被清空时，建议从头开始创建一个新的上下文窗口，而不是采用合并数据的方式。
  - “输入 `pwd`；你只能在这个目录内读取和写入文件。”
  - “请查看 `progress.txt、tests.json` 文件以及 `git` 日志。”
  - 在开始实现新功能之前，务必先手动执行一次基本的集成测试。
- 提供验证工具：随着自主执行任务的复杂性不断增加，LLM 需要能够在没有人类持续干预的情况下自行验证任务的正确性。像 `Playwright MCP` 服务器这类工具，或是用于测试用户界面的各种计算机功能，都能发挥重要作用。
- 鼓励充分利用上下文信息：提示 LLM 在继续处理之前，先完整地完成各个组成部分的处理。
```text
This is a very long task, so it may be beneficial to plan out your work clearly. It's encouraged to spend your entire output context working on the task - just make sure you don't run out of context with significant uncommitted work. Continue working systematically until you have completed this task.

这是一项非常耗时的任务，因此建议你清晰地规划好工作流程。鼓励你在整个输出上下文中持续投入完成任务，只需确保不会因未完成的重要工作而耗尽上下文资源。请系统性地继续工作，直到完成此任务为止。
```

---

## State management best practices    状态管理的最佳实践
- 在处理结构化数据时，请使用相应的格式：在记录测试结果或任务状态等结构化信息时，建议使用 `JSON` 或其他结构化格式，以便 LLM 能够准确理解数据的格式要求。
- 在记录进展时可以使用非结构化文本：自由形式的记录方式非常适合用来追踪整体进展和各项相关情况。
- 使用 `Git` 来跟踪状态：`Git` 能够记录所有操作记录和可恢复的节点状态。在利用 `Git` 来跨会话跟踪状态方面，LLM 的最新模型表现得尤为出色。
- 强调循序渐进的进展：明确要求 LLM 记录自身的进展情况，专注于逐步完成各项任务。
- 例子【状态追踪】:
```json
// Structured state file (tests.json)
{
  "tests": [
    { "id": 1, "name": "authentication_flow", "status": "passing" },
    { "id": 2, "name": "user_management", "status": "failing" },
    { "id": 3, "name": "api_endpoints", "status": "not_started" }
  ],
  "total": 200,
  "passing": 150,
  "failing": 25,
  "not_started": 25
}
```
```text
// Progress notes (progress.txt)
Session 3 progress:
- Fixed authentication token validation
- Updated user model to handle edge cases
- Next: investigate user_management test failures (test #2)
- Note: Do not remove tests as this could lead to missing functionality

第3阶段进展：  
- 修复了身份验证令牌的验证问题  
- 更新用户模型，以处理边缘情况  
- 下一步：调查 user_management 测试失败（测试编号2）  
- 注意：请勿删除测试用例，否则可能导致功能遗漏
```

---

## Balancing autonomy and safety    在自主性与安全性之间寻求平衡
如果没有适当的指导，LLM 可能会做出一些难以撤销的操作，或者对共享系统造成不良影响。
例如，它可能会删除文件、强制推送数据或向外部服务发送内容。
如果你希望 LLM 在采取可能带来风险的行动之前先进行确认，请在指令中添加相应的指导说明。
```text
Consider the reversibility and potential impact of your actions. You are encouraged to take local, reversible actions like editing files or running tests, but for actions that are hard to reverse, affect shared systems, or could be destructive, ask the user before proceeding.

Examples of actions that warrant confirmation:
- Destructive operations: deleting files or branches, dropping database tables, rm -rf
- Hard to reverse operations: git push --force, git reset --hard, amending published commits
- Operations visible to others: pushing code, commenting on PRs/issues, sending messages, modifying shared infrastructure

When encountering obstacles, do not use destructive actions as a shortcut. For example, don't bypass safety checks (e.g. --no-verify) or discard unfamiliar files that may be in-progress work.

请考虑您所采取行动的可逆性及其潜在影响。建议您采取本地且可逆的操作，例如编辑文件或运行测试；但对于难以撤销、可能影响共享系统或具有破坏性的操作，请在继续之前先征得用户同意。
需要确认的操作示例：
- 损害性操作：删除文件或分支、删除数据库表、rm -rf
- 难以回滚的操作：git push --force、git reset --hard、修改已发布的提交
- 可被他人查看的操作：推送代码、在 PR/问题上添加评论、发送消息、修改共享基础设施
遇到障碍时，不要用破坏性操作作为捷径。例如，不要绕过安全检查（如 --no-verify），也不要丢弃可能包含正在进行工作的未知文件。
```

---

## Research and information gathering   研究与信息收集
LLM 提供了出色的信息检索能力，为了能有效从多个来源整合结果，有以下几点需要注意
- **明确成功的标准**：明确什么才算是对研究问题的有效回答。
- **鼓励对信息来源进行核实**：请 LLM 从多个来源来验证信息是否准确。
- 对于复杂的科研任务，应采用结构化的处理方式：
```text
Search for this information in a structured way. As you gather data, develop several competing hypotheses. Track your confidence levels in your progress notes to improve calibration. Regularly self-critique your approach and plan. Update a hypothesis tree or research notes file to persist information and provide transparency. Break down this complex research task systematically.

以结构化的方式查找所需信息。在收集数据的过程中，提出多个相互竞争的假设。通过跟踪进展记录中的信心水平来提高校准能力。定期自我反思方法和计划。更新假设树或研究笔记文件，以持续保存信息并保持透明度。系统地分解这一复杂的研究任务。
```

---

## Subagent orchestration  子代理协调机制
当需要多个子代理来完成一个复杂的任务时，要注意一下几点
- **确保子代理工具的定义清晰明确**：子代理工具必须已经准备就绪，并在其定义中有所说明。
- **让 LLM 自行安排一切**：无需明确指示，LLM 会自行做出恰当的安排。
- **注意过度使用的问题**：LLM 特别倾向于使用子代理来处理任务。在那些用更简单、直接的方法就能解决问题的情况下，该模型仍会生成子代理。例如，当直接使用 `grep` 命令就能更快地完成代码搜索任务时，该模型却会生成子代理来处理这项任务。
如果发现子代理的使用频率过高，请明确说明在何种情况下可以使用子代理，而在何种情况下则不应使用：
```text
Use subagents when tasks can run in parallel, require isolated context, or involve independent workstreams that don't need to share state. For simple tasks, sequential operations, single-file edits, or tasks where you need to maintain context across steps, work directly rather than delegating.

当任务可以并行执行、需要独立上下文，或涉及无需共享状态的独立工作流时，可使用子代理。对于简单任务、顺序操作、单文件编辑，或需要在步骤间保持上下文的任务，应直接处理，而非委托。
```

---

## Chain complex prompts  链式复合提示/链状复合指令
最常见的链式处理模式是自我修正：
- 先生成初稿，然后让 LLM 根据既定标准对初稿进行审核，之后再根据审核结果对初稿进行进一步修改。
- 每个步骤都对应一次 API 调用，因此你可以随时记录日志、进行评估或选择不同的处理路径。

---

## Avoid focusing on passing tests and hard-coding    避免只专注于通过考试和死记硬背。
LLM 有时会过于专注于让测试通过，而忽视了更通用的解决方案。或者，对于复杂的重构任务，他可能会使用辅助脚本来临时解决问题，而不是直接使用标准工具。为避免这种情况，确保解决方案的稳健性和通用性，有必要采取相应的措施：
```text
Please write a high-quality, general-purpose solution using the standard tools available. Do not create helper scripts or workarounds to accomplish the task more efficiently. Implement a solution that works correctly for all valid inputs, not just the test cases. Do not hard-code values or create solutions that only work for specific test inputs. Instead, implement the actual logic that solves the problem generally.

Focus on understanding the problem requirements and implementing the correct algorithm. Tests are there to verify correctness, not to define the solution. Provide a principled implementation that follows best practices and software design principles.

If the task is unreasonable or infeasible, or if any of the tests are incorrect, please inform me rather than working around them. The solution should be robust, maintainable, and extendable.

请使用现有的标准工具编写高质量的通用解决方案。不要创建辅助脚本或绕过方案来更高效地完成任务。实现一个对所有有效输入都正确工作的解决方案，而不仅仅是针对测试用例。不要硬编码值，也不要创建仅适用于特定测试输入的解决方案。相反，应实现能够普遍解决该问题的实际逻辑。
专注于理解问题需求并实现正确的算法。测试的目的是验证正确性，而非定义解决方案。应提供遵循最佳实践和软件设计原则的合理实现方案。
如果任务不合理或不可行，或者任何测试存在错误，请及时告知我，而不是试图绕过问题。解决方案应具备健壮性、可维护性和可扩展性。
```

---

## Minimizing hallucinations in agentic coding    在智能体编码过程中尽量减少幻觉现象
```text
<investigate_before_answering>
Never speculate about code you have not opened. If the user references a specific file, you MUST read the file before answering. Make sure to investigate and read relevant files BEFORE answering questions about the codebase. Never make any claims about code before investigating unless you are certain of the correct answer - give grounded and hallucination-free answers.
</investigate_before_answering>

在回答之前，切勿对未打开的代码进行猜测。如果用户提到了某个特定文件，你必须先阅读该文件再作答。在回答关于代码库的问题前，请务必仔细调查并阅读相关文件。除非你确定答案正确，否则切勿在未调查的情况下对代码做出任何声称——请提供有依据、不含臆测的回答。
```

---

# Capability-specific tips  针对各项能力的具体建议/提示
## Frontend design  前端设计
以下是一个可用于促进更好前端设计的系统提示语示例：
```text
<frontend_aesthetics>
You tend to converge toward generic, "on distribution" outputs. In frontend design, this creates what users call the "AI slop" aesthetic. Avoid this: make creative, distinctive frontends that surprise and delight.

Focus on:
- Typography: Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial and Inter; opt instead for distinctive choices that elevate the frontend's aesthetics.
- Color & Theme: Commit to a cohesive aesthetic. Use CSS variables for consistency. Dominant colors with sharp accents outperform timid, evenly-distributed palettes. Draw from IDE themes and cultural aesthetics for inspiration.
- Motion: Use animations for effects and micro-interactions. Prioritize CSS-only solutions for HTML. Use Motion library for React when available. Focus on high-impact moments: one well-orchestrated page load with staggered reveals (animation-delay) creates more delight than scattered micro-interactions.
- Backgrounds: Create atmosphere and depth rather than defaulting to solid colors. Layer CSS gradients, use geometric patterns, or add contextual effects that match the overall aesthetic.

Avoid generic AI-generated aesthetics:
- Overused font families (Inter, Roboto, Arial, system fonts)
- Clichéd color schemes (particularly purple gradients on white backgrounds)
- Predictable layouts and component patterns
- Cookie-cutter design that lacks context-specific character

Interpret creatively and make unexpected choices that feel genuinely designed for the context. Vary between light and dark themes, different fonts, different aesthetics. You still tend to converge on common choices (Space Grotesk, for example) across generations. Avoid this: it is critical that you think outside the box!
</frontend_aesthetics>

你往往倾向于采用通用的、标准化的输出。在前端设计中，这会形成用户所说的“AI粗糙”风格。请避免这种情况：打造富有创意且独具特色的前端界面，带来惊喜与愉悦。
重点关注以下方面：
- 字体设计：选择美观、独特且富有吸引力的字体，避免使用Arial或Inter等通用字体，转而采用更具个性的设计，以提升前端的视觉美感。
- 颜色与主题：保持整体风格的一致性。通过CSS变量确保统一性。鲜明的主色调搭配清晰的点缀效果，比柔和均匀的配色方案更具表现力。可从IDE主题和文化美学中获取灵感。
- 动画效果：在交互中运用动画实现视觉效果和微交互。优先考虑仅使用CSS实现HTML功能，如可用Motion库处理React项目。注重高冲击力的瞬间体验：一次精心编排的页面加载，配合渐进式展示（animation-delay），带来的愉悦感远胜于零散的微交互。
- 背景设计：创造氛围与层次感，而非依赖纯色背景。叠加CSS渐变、使用几何图案，或添加与整体美学相契合的上下文特效。
避免使用通用的AI生成美学：
- 过度使用的字体（如Inter、Roboto、Arial、系统字体）
- 被广泛采用的配色方案（尤其是白色背景上的紫色渐变）
- 预测性强的布局和组件模式
- 缺乏情境特异性特征的千篇一律设计
富有创意地诠释，并做出看似为特定情境量身打造的意外选择。在浅色与深色主题、不同字体和视觉风格之间进行灵活切换。然而，你仍可能在不同世代中倾向于采用相似的选择（例如Space Grotesk）。请避免这种情况：关键在于跳出常规思维！
```

# OpenAI 
## 前端设计 
建议库：
```text
Styling / UI: Tailwind CSS, shadcn/ui, Radix Themes
样式设计/用户界面：Tailwind CSS、shadcn/ui、Radix Themes
Icons: Lucide, Material Symbols, Heroicons
图标风格：清晰风格、材质风格、英雄图标风格
Animation: Motion  动画：运动效果
```
在处理规模较大的代码库时的前端开发工作中，在提示语中加入这些类别的指令，能够取得最佳效果：
```text

原则：确立视觉质量标准，使用可模块化/重复使用的组件，保持设计的统一性。
UI/UX 设计：需明确指定文字样式、颜色、间距/布局、各种交互状态下的表现形式（如鼠标悬停时、页面为空时、加载中的状态），以及产品的无障碍性设计。
结构：确定文件/文件夹的布局，以便实现无缝集成。
组件：请举例说明可重用的封装方式，以及后端调用与前端逻辑分离的策略。
页面：提供各种常见布局的模板。
代理指令：要求模型确认设计假设、构建项目框架、执行各项标准、实现 API 集成、测试系统状态并记录代码相关文档。
```

---

## 复杂任务
涉及 LLM 的复杂且耗时的实施过程中，应重点关注以下三个关键方面：
- 首先，要充分规划各项任务，确保问题能够得到彻底解决；
- 其次，在使用重要工具之前，需提前明确相关的操作步骤；
- 最后，应使用任务管理工具来有条不紊地跟踪工作流程和进度。
```text
Remember, you are an agent - please keep going until the user's
query is completely resolved, before ending your turn and yielding
back to the user. Decompose the user's query into all required
sub-requests, and confirm that each is completed. Do not stop
after completing only part of the request. Only terminate your
turn when you are sure that the problem is solved. You must be
prepared to answer multiple queries and only finish the call once
the user has confirmed they're done.

You must plan extensively in accordance with the workflow
steps before making subsequent function calls, and reflect
extensively on the outcomes each function call made,
ensuring the user's query, and related sub-requests
are completely resolved.

请记住，您是代理，务必继续处理，直到用户的问题完全解决为止，然后再结束您的回合，将控制权交还给用户。
将用户的请求分解为所有必要的子请求，并确认每个子请求均已完成。
不要在仅完成部分请求后就停止。只有在确定问题已解决时，才应结束您的回合。您必须准备好回答多个问题，并且只有在用户确认完成之后，才能结束本次通话。
在进行后续函数调用之前，必须根据工作流程步骤进行充分规划，并对每次函数调用的结果进行深入反思，确保用户的查询及相关子请求得到完全解决。
```

---

## Preambles for transparency   实现透明度的必要前提/实现透明度的基础条件
要求模型解释为何要调用某个工具，但只需在关键的步骤中进行说明即可。
```text
Before you call a tool explain why you are calling it

在调用工具之前，请先说明您为何要调用它。
```

