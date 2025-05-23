
**核心设计原则 (贯穿所有阶段):**

*   **统一性 (Unity):** 跨平台（示波器嵌入式网页、PC Web、移动端/小程序、AR）提供一致的核心体验和视觉语言，但根据平台特性优化交互。
*   **情境感知 (Context-Aware):** AI 必须理解当前任务、示波器状态、历史交互，提供相关性强的辅助。
*   **用户控制与信任 (Control & Trust):** 特别是在控制示波器或硬件时，用户需要清晰了解 AI 的意图，有明确的确认/取消机制，并能随时接管。
*   **反馈清晰 (Clear Feedback):** AI 的状态、操作过程、结果都需要实时、明确地反馈给用户。
*   **效率优先 (Efficiency):** 简化操作流程，减少用户认知负荷，尤其是在专业测试场景下。
*   **高设计感 (Aesthetics):** 现代、简洁、专业、科技感。

---

## 阶段 2：AI Agent 控制系统 (增强版工业应用)

此阶段显著增强了 AI Agent 的能力，专注于满足工业测试环境的需求，包括自动化序列、合规性、安全性、报告生成以及 SCPI 脚本生成辅助。

**核心功能:**

1.  **自然语言到安全控制:** 将用户的文本/语音指令转化为安全、合规的仪器控制命令（主要是 SCPI），用于直接执行。
2.  **自动化测试执行:** 加载、管理（启动、暂停、停止、恢复）并执行预定义的工业测试序列（例如，合规性测试、QC 流程、特定的分析程序）。
3.  **SCPI 脚本生成与辅助:** 根据用户需求，生成基于 Python（使用如 PyVISA 等库）或其他指定语言的脚本，用于仪器控制和测试自动化。
4.  **脚本验证支持:** 通过执行语法检查、SCPI 命令有效性检查（基于仪器型号知识）以及识别潜在逻辑问题或风险，辅助用户验证生成或提供的脚本。
5.  **标准化报告:** 基于模板自动生成全面的测试报告，包括配置、结果、通过/失败判定、仪器信息、时间戳和可选的操作员信息。

**输入:**

*   **自然语言指令 (文本/语音):**
    *   直接仪器控制: "设置通道 1 为 1V/div", "在通道 2 的上升沿 500mV 处触发"。
    *   基于标准的测试: "运行 EN 50160 电压暂降抗扰度测试"。
    *   序列执行: "执行用于板子版本 2.1 的 QC 序列 #7"。
    *   分析请求: "计算采集波形的 RMS 值", "对通道 3 执行 FFT 分析"。
    *   **脚本生成请求:** "编写一个 Python 脚本来测量不同负载下的电源纹波", "生成 PyVISA 代码来配置差分探头并捕获 I2C 流量"。
*   **测试序列选择:** 从库或文件中选择预定义的序列。
*   **脚本输入 (用于验证):** 粘贴或上传 Python/SCPI 脚本代码。
*   **(隐式) 仪器型号选择:** 用户指定或系统检测目标仪器型号，以确保准确的 SCPI 生成/验证。

**输出:**

*   **确认与安全提示:**
    *   在执行*任何*可能影响外部设备、涉及安全风险（如高压）或启动长时间自动化序列的命令之前，**必须**有强制性的确认请求。
    *   确认信息包含清晰的风险描述，并可能需要用户采取特定操作（例如，勾选确认框）。
    *   通过 AI 界面执行*生成的脚本*时，需要单独的、强化的确认。
*   **执行状态与进度:**
    *   清晰指示当前操作（例如，“正在配置触发”，“正在运行序列步骤 3/10”）。
    *   实时显示测试序列进度（已完成步骤、当前步骤、每步的通过/失败状态、预计剩余时间）。
*   **结果反馈:**
    *   清晰显示测量值。
    *   与标准限值或定义阈值的比较，包括计算出的裕量/差异。
    *   醒目的通过/失败 (Pass/Fail) 指示器（整体和每步/测量）。
    *   生成的工件，如波形截图、数据日志或分析图。
*   **生成的脚本:**
    *   在带有语法高亮的代码查看器/编辑器中显示 Python（或其他指定语言）代码。
*   **脚本验证结果:**
    *   关于语法正确性的反馈。
    *   针对目标仪器型号的 SCPI 命令有效性/兼容性报告。
    *   高亮显示潜在问题（例如，缺少错误检查、无限循环、危险命令、缺少同步等待）。
*   **标准化测试报告:**
    *   生成的文档（PDF、CSV、HTML），包含测试配置、结果、判定、仪器信息、时间戳、（可选）操作员 ID 以及可能相关的脚本片段。
*   **部署状态 (脚本):** 确认操作，如保存到文件、添加到库或发送到目标。
*   **审计日志:** 可访问的操作记录，包含用户指令、AI 操作、仪器响应、关键结果和确认信息。

**AI Agent 能力 (增强版工业):**

*   **意图理解:** 解码用户意图，涵盖直接控制、序列执行、分析、脚本生成和验证。
*   **标准感知:** 理解特定工业标准（例如，EN 50160、汽车以太网、电能质量）中定义的流程、参数和限值。
*   **测试序列管理:** 加载、解释、执行、监控和管理复杂的测试序列（步骤、循环、条件分支）。
*   **合规性判断:** 自动将测量结果与定义的限值（来自标准或用户输入）进行比较，并提供通过/失败判定。
*   **SCPI 知识库:** 维护一个包含相关仪器的 SCPI 命令、语法、参数和型号特定变体的数据库。
*   **安全与合理性检查:** 在执行前进行基本验证（例如，电压范围与探头设置）。标记潜在危险操作。
*   **Python (PyVISA) 脚本生成:** 创建结构良好、可读性强的 Python 脚本，用于仪器控制，并融入基本的错误处理和同步最佳实践（例如，使用 `*OPC?`）。
*   **脚本验证引擎:**
    *   执行静态代码分析 (Linting) 以检查语法。
    *   对照知识库检查 SCPI 命令在目标型号上的有效性。
    *   应用启发式方法识别基本的逻辑缺陷或常见风险（例如，命令后缺少等待、可能有害的系统命令如 `:SYSTem:PRESet` 未加警告）。
*   **报告生成:** 利用模板将测试数据组装成标准化的报告格式。
*   **部署处理:** 方便保存、复制或传输生成的脚本。
*   **多轮交互:** 进行对话以澄清需求或细化复杂请求（尤其是在脚本生成时）。
*   **状态查询:** 能够查询仪器状态以辅助决策或报告当前设置。

**安全性 (工业重点):**

*   **操作隔离:** 清晰区分仅影响内部设置的命令与控制外部输出或连接的命令。
*   **多级确认:** 对高风险操作、长时间自动化测试、潜在破坏性命令或通过 AI 界面启动的脚本执行，实施更严格的确认协议。
*   **生成与执行分离:** 强调 AI 主要负责*生成*和*辅助验证*脚本，但**用户承担审查和执行脚本的最终责任**，尤其是在关键环境中。由 AI 发起的脚本执行需要明确、知情的同意。
*   **验证局限性免责声明:** 明确告知用户 AI 验证虽有帮助但并非详尽无遗，不能保证脚本完全正确或无害。
*   **风险高亮:** 在生成的脚本或提议的操作计划中，识别并明确警告潜在的风险命令或逻辑。
*   **紧急停止 (E-Stop):** 提供一个高度可见、易于访问且能立即生效的“紧急停止”机制，以中止所有正在进行的由 AI 驱动的控制操作和通过 AI 启动的脚本执行。

**平台:**

*   **主要:** 网页界面 (PC 端提供全部功能，包括脚本编写；嵌入式示波器端用于序列执行、状态监控、基本控制)。
*   **次要:** 移动应用 (主要用于远程监控测试状态、接收通知/结果，可能可以启动预先批准的序列)。

**UX 设计 (阶段 2 - 增强版工业):**

*   **核心用户流程:**
    1.  **直接控制 / 序列执行:** 用户输入 (指令/选择序列) -> AI 解析/加载 -> AI 展示计划/预览 (含步骤、安全警告) -> 用户审核并确认 (可能需多级确认) -> AI 执行 -> 实时状态/进度/中间结果反馈 -> 最终结果 & 报告生成选项。
    2.  **脚本生成与验证:** 用户描述脚本需求 -> AI 生成脚本 -> AI 显示脚本 (在代码编辑器中) & 初步验证反馈 -> 用户审查/修改 -> (可选) 用户请求深度验证 -> AI 提供详细反馈 -> 用户最终确认脚本 -> 用户选择部署选项 (保存、复制、添加到库) **或** (在强警告和确认下) 通过 AI 启动执行。
*   **UI 界面设计:**
    *   **专用视图/模式:** 为“合规性测试”、“QC 自动化”、“脚本开发”等任务提供特定 UI，优化布局。清晰标示当前操作模式（直接控制 vs. 脚本编写）。
    *   **操作计划/预览:** 详细说明提议的操作或测试序列步骤。高亮影响外部系统的命令。显示序列进度（总步骤、当前步骤、预计时间）。
    *   **安全确认对话框:** 使用明确无误的语言，详细说明风险，要求用户明确操作（例如，勾选“我已核实安全连接”复选框）。
    *   **集成代码编辑器/查看器:** 用于显示/编辑生成的脚本。功能包括语法高亮、行号、复制功能，可能还有基础编辑功能。
    *   **脚本验证显示:** 清晰展示验证结果（语法、SCPI 检查、建议、风险），尽可能链接到具体的代码行。
    *   **状态指示器:** 全面显示，包括仪器状态、活动测试名称、进度条、当前步骤状态（等待、运行中、通过、失败、跳过），以及醒目的序列整体通过/失败指示器。
    *   **结果显示:** 显示测量值及其对应的目标限值、计算出的裕量/差异 (%)，高亮显示失败项（例如，红色背景/图标）。支持波形与模板/黄金样本叠加比较。
    *   **报告生成界面:** 简单的“生成报告”按钮、模板选择、预览选项、多种导出格式（PDF、CSV、HTML）。
    *   **部署选项:** 清晰的按钮/菜单，如“另存为...”、“复制脚本”、“添加到库”、“发送至...”。
    *   **脚本执行按钮 (若提供):** 在视觉上与其他按钮区分（颜色/图标），点击后触发多阶段确认对话框，解释风险并要求用户明确同意。
    *   **紧急停止 (E-Stop) 按钮:** 始终可见、易于点击的按钮，标记为“停止 AI”或“紧急停止”。点击后立即生效。
    *   **仪器型号选择:** 允许用户指定目标设备，以实现准确的 SCPI 和验证。
    *   **审计日志访问:** 提供访问详细操作日志的链接或区域。

**示例场景 (增强版工业):**

1.  **场景: 电源效率测试 (直接控制 - PC 网页)**
    *   *用户:* "测量效率：Ch1 Vin (标称 230V)，Ch2 Iin (使用 1A/V 探头)，Ch3 Vout (标称 12V)，Ch4 Iout (使用 10A/V 探头)。"
    *   *AI:* "好的，正在配置效率测量。计划：1. 设置 Ch1/Ch3 V/div。2. 根据探头灵敏度设置 Ch2/Ch4 V/div。3. 启用功率分析。4. 计算效率 (Pout/Pin)。**⚠️ 安全警告：确保所有探头连接正确（特别是电流探头的极性和额定值）并保持安全操作距离。**" [显示详细的 SCPI 计划]。**[确认并继续 (已检查安全)] [修改] [取消]**
    *   *用户:* 点击 [确认并继续]。
    *   *AI:* (状态更新) "正在配置... 正在测量..." -> (结果气泡) "Vin=230.1V, Iin=0.45A(rms), Pin=98W(PF=0.94)。Vout=12.05V, Iout=7.8A(dc), Pout=94W。**效率: 95.9%**" [包含相关图表缩略图]。

2.  **场景: 生产 QC 测试 (序列执行 - 示波器网页)**
    *   *操作员:* 从下拉菜单中选择 "QC 序列 #3: 信号完整性"。
    *   *AI:* 显示 "QC 序列 #3"。步骤 1/5: "将探头连接到 TP5 (时钟)"。**[开始测试]**
    *   *操作员:* 连接探头，点击 [开始测试]。
    *   *AI:* (UI 更新) 进度: 步骤 1/5 运行中: "测量频率"。-> 结果: "频率: 50.01 MHz (限值: 49.9-50.1 MHz)" **通过 (PASS)**。-> 步骤 2/5 运行中: "测量上升时间"... 自动继续。
    *   *AI (结束):* 整体结果: **失败 (FAIL)** (步骤 4 失败)。**[查看详情] [保存报告] [测试下一单元]**

3.  **场景: 自动化测试脚本生成 (研发 - PC 网页)**
    *   *工程师:* "为地址是 'TCPIP0::192.168.1.10::INSTR' 的示波器 (型号 MSOZ254A) 生成一个 Python PyVISA 脚本。它应该设置 Ch1 用于 5V 逻辑 (1V/div)，在 2.5V 的下降沿触发，以 HiRes 模式采集 10ms 的数据，然后测量找到的第一个负脉冲的脉冲宽度。"
    *   *AI:* "正在为 MSOZ254A 生成 Python 脚本以测量脉冲宽度..." [在编辑器中显示 Python 代码]。
    *   *AI (验证面板):*
        *   ✅ Python 语法: OK
        *   ✅ SCPI 检查 (MSOZ254A): 命令 `:CHAN1:SCAL`, `:TRIG:EDGE:SOUR`, `:TRIG:LEV`, `:ACQ:MODE HRES`, `:ACQ:SRAT` (示例，实际命令可能不同), `:MEAS:PULS:WIDT` 对 MSOZ254A 似乎有效。
        *   💡 建议: 在开始采集后添加了 `scope.query('*OPC?')` 以确保在测量前完成采集。
        *   💡 建议: 包含了 `try...except...finally` 用于资源管理。
        *   ❗️ 风险: 脚本会修改仪器状态。请仔细检查测量命令 (`:MEAS:PULS:WIDT`) 的参数是否适用于您的具体信号。
    *   *工程师:* 审查代码，可能在编辑器中修改或复制它。点击 **[另存为...]** 或 **[复制到剪贴板]**。

---
