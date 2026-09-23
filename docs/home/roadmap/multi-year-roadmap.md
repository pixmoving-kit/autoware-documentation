<a id="autoware-multi-year-roadmap"></a>

# Autoware 多年路线图

AWF 根据愿景、使命和核心价值观，制定了希望实现的关键目标与多年目标。随后将以此为基础制定执行计划，明确如何达成预期目标。

目前已定义 8 项关键目标，覆盖技术、会员与开源社区、品牌三个主题。每项关键目标均设定了三年期间（Y1–Y3）的阶段目标。

<a id="key-objectives-and-goals"></a>

## 关键目标与阶段目标

<a id="technology"></a>

### 技术

<a id="objective-1-incorporate-cutting-edge-technologies-in-autowares-software-as-part-of-an-ai-first-approach"></a>

#### 目标 1：以 AI 优先的方式，将前沿技术引入 Autoware 软件

- **第 1 年：**
  - 基于激光雷达、视觉和毫米波雷达等关键自动驾驶传感器，引入组件化 AI 感知栈。
  - 开发基于 AI 的规划器初版，实现全链路由 AI 组件构成的自动驾驶软件栈；通过内部开发及集成会员企业技术来完成。
- **第 2 年：**
  - 推出使用单一神经网络的端到端自动驾驶软件栈初版，直接处理传感器数据并计算安全行驶轨迹。
- **第 3 年：**
  - 推出完整的混合 AI 软件栈，以端到端自动驾驶为主要模式，同时并行运行组件化 AI 软件栈，提供额外安全保障并作为基础技术层。

<a id="objective-2-develop-autowares-software-towards-production-ready-solutions-that-can-be-more-easily-commercialized-by-members"></a>

#### 目标 2：推动 Autoware 软件达到可量产水平，便于会员实现商业化

- **第 1 年：**
  - 推出与 OpenADKit 紧密集成的可量产 Autoware 初版，面向低速自动驾驶（仓库、货运、地理围栏环境）和私家车（高速公路自动驾驶），达到 TRL-6。
- **第 2 年：**
  - 根据 Autoware 会员与试点合作伙伴的反馈，继续开发面向低速自动驾驶和私家车的可量产 Autoware，达到 TRL-7。
  - 推出与 OpenADKit 紧密集成的可量产 Autoware 初版，面向自动驾驶公交（城市道路、固定路线）和非道路车辆（采矿、物流、非道路运输等），达到 TRL-6。
- **第 3 年：**
  - 根据 Autoware 会员与试点合作伙伴的反馈，继续开发面向低速自动驾驶和私家车的可量产 Autoware，达到 TRL-8。
  - 根据 Autoware 会员与试点合作伙伴的反馈，继续开发面向自动驾驶公交和非道路车辆的可量产 Autoware，达到 TRL-7。
  - 推出与 OpenADKit 紧密集成、面向 Robotaxi 的可量产 Autoware 初版，达到 TRL-6。

<a id="objective-3-increase-deployments-of-autowares-software-to-a-greater-number-of-vehicles-driving-more-miles-autonomously"></a>

#### 目标 3：将 Autoware 软件部署到更多车辆上，增加自动驾驶里程

- **第 1 年：**
  - 与至少一家核心企业达成试点合作，在真实环境中的单台车辆上测试达到 TRL-6 的可量产 Autoware 软件栈，面向低速自动驾驶和私家车。低速自动驾驶的潜在核心企业包括 Eve Autonomy、Whale Dynamic 和 Robeff；私家车方向包括 AMD、DeepenAI 和 AutoCore.ai。
- **第 2 年：**
  - 与至少一家核心企业达成试点合作，在真实环境中以多车小规模部署，测试达到 TRL-7 的低速自动驾驶及私家车可量产 Autoware 软件栈。
  - 与至少一家核心企业达成试点合作，在真实环境中的单台车辆上测试达到 TRL-6 的自动驾驶公交及非道路车辆可量产 Autoware 软件栈。自动驾驶公交的潜在核心企业为 ADASTEC，非道路车辆方向为 DriveBlocks。
- **第 3 年：**
  - 与至少一家核心企业达成试点合作，在真实环境中以更大规模车队部署，测试达到 TRL-8 的低速自动驾驶及私家车可量产 Autoware 软件栈。
  - 与至少一家核心企业达成试点合作，在真实环境中以多车小规模部署，测试达到 TRL-7 的自动驾驶公交及非道路车辆可量产 Autoware 软件栈。
  - 与至少一家核心企业达成试点合作，在真实环境中的单台车辆上测试达到 TRL-6 的 Robotaxi 可量产 Autoware 软件栈；潜在核心企业包括 TIER IV。

<a id="membership-open-source-community"></a>

### 会员与开源社区

<a id="objective-4-improve-the-ease-with-which-new-users-can-get-started-with-autoware-and-begin-open-source-contributions-and-integration-of-autoware-technologies-within-their-pre-existing-software-stack"></a>

#### 目标 4：帮助新用户更容易上手 Autoware、参与开源贡献，并将 Autoware 技术集成到现有软件栈

- **第 1 年：**
  - 更新 Autoware 文档、视频教程和入门指南，帮助用户更容易理解如何开始使用 Autoware。
  - 通过调查衡量新用户满意度，评估入门流程的便利程度。
  - 为 Autoware 软件版本引入清晰的版本编号与命名，使 LTS 版本与文档、教程紧密关联。
- **第 2 年：**
  - 为所有 Autoware 功能包建立类似 ROS Index 的列表，提供可搜索目录，包含版本、发布日期、维护者、描述和要求等详情。
  - 创建带图形界面的 Autoware 配置面板工具，便于启动、调试和管理 Autoware 自动驾驶软件栈。
- **第 3 年：**
  - 创建 Autoware 远程监控系统的概念验证初版，在在线仪表盘中记录车辆 GPS 位置、传感器日志、GPS 基础遥测、实时相机视频流及其他简单信息。
  - 研究将 V2X 消息和车载信息娱乐相关信息集成到远程监控系统的方法。

<a id="objective-5-grow-autowares-open-source-community-especially-premium-members-and-individual-contributors"></a>

#### 目标 5：扩大 Autoware 开源社区，尤其是高级会员和个人贡献者

- **第 1 年：**
  - 制定并实施面向高级会员和个人贡献者的双层营销计划，通过加强社交媒体推广（重点是视频内容，以 YouTube 和 LinkedIn 为主要平台）、新闻稿（CISION 等）、内容营销（博客与月度通讯，使用 AWF 网站、Medium、Substack 等）、SEO、与科技意见领袖合作（体验或展示搭载 AWF 技术的车辆，可带来大量观看），以及在开发者密集的平台（如 Reddit、Discord、HackerNews）参与交流，吸引更多成员。
  - 新增至少 12 家高级会员。
  - 新增至少 25 名不隶属于高级会员企业、积极参与 Autoware 的开源贡献者（参加工作组会议并贡献代码）。
  - AWF 所有仓库的 GitHub star 总数超过 15,000。
- **第 2 年：**
  - 持续执行双层营销计划，吸引更多高级会员和个人贡献者。
  - 新增至少 20 家高级会员。
  - 新增至少 40 名不隶属于高级会员企业、积极参与 Autoware 的开源贡献者（参加工作组会议并贡献代码）。
  - AWF 所有仓库的 GitHub star 总数超过 20,000。
- **第 3 年：**
  - 持续执行双层营销计划，吸引更多高级会员和个人贡献者。
  - 新增至少 30 家高级会员。
  - 新增至少 60 名不隶属于高级会员企业、积极参与 Autoware 的开源贡献者（参加工作组会议并贡献代码）。
  - AWF 所有仓库的 GitHub star 总数超过 25,000。

<a id="objective-6-improve-engagement-with-autoware-members-and-the-community-at-large-for-a-tight-feedback-loop-which-helps-foster-stronger-collaboration-and-active-participation-by-the-community"></a>

#### 目标 6：加强与 Autoware 会员及整个社区的互动，形成紧密反馈循环，促进协作和积极参与

- **第 1 年：**
  - 指定高级会员联络负责人，定期与各高级会员开会，推动其积极参与，并跟踪其承诺的 AWF 计划、社区贡献及在自身应用中使用 Autoware 的情况。
  - 再次举办设有奖金的 Autoware 技术挑战赛，鼓励提交高水平研发成果，解决 Autoware 最迫切的技术难题。
  - 举办首届 Autoware 年度颁奖典礼（类似 ROS Awards），表彰和奖励个人开源贡献者及会员企业，打造“Autoware 奥斯卡”。例如，可根据提交数量、修复缺陷数量或工作组主席认可的投入，向个人颁发证书或奖金；也可根据实地部署、社区参与、开源代码贡献等方面的贡献，向会员企业颁发奖金。
- **第 2 年：**
  - 举办每月全体交流会及其他活动（例如项目推介、企业展示），增加与高级会员的直接沟通，让其与基金会整体展开交流。
  - 每月举办更适合个人开发者的活动、黑客松、在线讲座、交流、成果展示和教程活动，扩大开源贡献者群体。
  - 持续举办 Autoware 技术挑战赛和年度颁奖典礼。
- **第 3 年：**
  - 举办首届为期多日的 Autoware 年度大会，面向会员企业、整个开源社区、潜在新会员、其他基金会、初创企业和个人开发者等，涵盖成果展示、推介活动、专题讨论和黑客松。
  - 持续举办每月活动，加强与高级会员和开源贡献者的互动。
  - 持续举办 Autoware 技术挑战赛和年度颁奖典礼。

<a id="objective-7-improve-networking-between-members-for-sharing-of-business-opportunities-enhancement-of-lead-generation-and-revenue-growth"></a>

#### 目标 7：加强会员之间的联系，分享商业机会、增加潜在客户并推动收入增长

- **第 1 年：**
  - 更新并重塑 Autoware.io 品牌，提供企业最新信息，并了解企业需求，例如硬件合作伙伴、演示合作伙伴、客户等。
  - 指定专人，根据企业需求组织企业间的对接活动。
- **第 2 年：**
  - 每月举办业务拓展活动，让 Autoware 企业推介产品、公开招募合作方并提高品牌知名度；允许所有人参加以扩大影响，例如近期在湾区举办的 DeepenAI/ASAM/PoV 工作组活动。
- **第 3 年：**
  - 举办首届 Autoware 加速器或资助计划，邀请初创企业围绕特定商业主题或项目，与核心 Autoware 会员合作，并提供现金奖励（理念类似 Autoware 挑战赛，但更侧重业务发展）。

<a id="brand"></a>

### 品牌

<a id="objective-8-improve-autowares-brand-positioning-as-a-market-leader-in-the-autonomous-driving-space"></a>

#### 目标 8：提升 Autoware 作为自动驾驶市场领导者的品牌定位

- **第 1 年：**
  - 配合重要公告（例如合作、活动、技术进展）发布新闻稿，并与专业公关公司合作或直接联系记者，争取至少一家顶级科技媒体报道，例如 Forbes、TechCrunch、WSJ、Sifted、Wired、Bloomberg 等。
  - 除 CES 外，至少参加并在一场知名科技活动中演讲，例如 WebSummit、MWC、SLUSH、SHIFT、Viva Technology、London TechWeek 等。
- **第 2 年：**
  - 制作高质量视频系列，通过访谈和图形展示知名 Autoware 会员企业（例如 ARM、TIER IV、AMD）如何使用 Autoware，以及已完成的演示，并通过社交媒体自然传播和付费推广，争取媒体报道。
- **第 3 年：**
  - 将 Autoware 技术栈提交到感知、规划、端到端自动驾驶等开放基准测试中，争取进入前列（第 1–3 名）。
