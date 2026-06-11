智能查房 隐私政策
生效日期：2026-06-11
最后更新：2026-06-11（政策版本 2）
应用版本：1.1.0
Bundle ID：seayeh.SmartWardRound2
运营主体：叶海波
客服邮箱：m1568_mbtqqsozoz@aka.yeah.net
在线地址：https://seayeh.github.io/privacy-policy/

引言
「智能查房」（App Store 显示名称亦可能为 SmartWardRound，以下简称「本应用」）是一款面向医疗从业者的本地优先病历书写与查房辅助工具。 开发者叶海波（以下简称「我们」）尊重并保护您的隐私。

本隐私政策依据本应用实际代码实现编写，说明我们在您使用本应用时如何收集、存储、使用、传输与删除相关信息。 本应用不向开发者自有服务器上传患者病历；仅在您主动启用相关功能时，将经脱敏处理的内容发送至您自行配置的第三方服务或同一局域网内的接收端。

本应用不用于广告追踪，不出售患者或用户数据，未集成第三方统计、广告或崩溃上报 SDK。

一、信息收集与存储
1.1 我们收集或处理的信息类型
患者标识与临床信息：您主动录入的床号、姓名、性别、年龄、住院号/病历号、入院/出院日期、主要诊断、入院处理与诊疗计划、手术相关信息、诊断修正记录等。
查房与文书数据：查房会话、白板医嘱、待办事项、病程记录、入院记录、出院/转科/术前等文书草稿与正文、辅助检查摘要等。
本地账号资料：工号（userId）、姓名、科室、职称、电话、头像（PNG，Base64 编码）、最近登录时间与设备名称。
语音与图像：您在查房或医患转写场景中主动录制的音频；您通过相机或相册选择的床头卡、腕带、检验检查报告等图像（用于本地 OCR 识别）。
AI 配置：您在「设置 → AI Key 管理」中自行录入的第三方大模型 API Key（仅存于本机 Keychain，不上传至开发者服务器）。
应用偏好与元数据：语言、科室模板、UI 偏好、局域网发送目标 IP、隐私政策同意版本等（不含患者明文 PHI）。
1.2 存储方式与位置
结论：患者病历与查房数据以本机存储为主；部分功能会在您主动使用时向第三方或局域网发送经脱敏的内容；本应用未启用 iCloud 病历库同步。

存储方式	存储内容	是否上传至开发者服务器
SwiftData 本地数据库	患者、查房会话、白板医嘱、病程等结构化临床数据	否（CloudKit 同步已关闭）
AES-GCM 加密文件（Application Support/ClinicalSensitiveData）	文书草稿、PHI 侧车信封、账号资料、离线 AI 队列、个人模板等	否
Keychain	AI API Key、账号密码摘要、加密密钥材料	否
UserDefaults	登录状态、应用偏好、AI 网关地址/模型名（不含 Key）、目标 PC IP 等	否
临时目录	PDF/Word 导出文件（分享前暂存）	否（仅通过系统分享离开本机）
本地审计日志	应用启动、删除等安全事件（不含病历正文）	否
临床敏感目录已标记为排除 iCloud 备份（在 Apple 系统策略允许范围内）。 若您在系统层面开启 iCloud 或设备整机备份，备份内容是否包含应用沙盒由 Apple 账户与系统设置决定，这不等同于本应用自行将病历上传至开发者服务器。

本应用未使用 Core Data CloudKit 同步、NSUbiquitousKeyValueStore 或 iCloud 病历库。

二、账户系统
本应用提供纯本地账户，用于在同一设备上区分不同使用者的工作区，不涉及远程注册、云端登录或 Sign in with Apple / 微信等第三方登录。

注册与登录：您在本机设置工号、姓名与密码（最少 6 位）；密码以 PBKDF 风格摘要存入 Keychain，不经开发者服务器验证。
账户资料：姓名、科室、职称、电话、头像等保存在本机加密存储中。
数据隔离：患者与查房会话按 ownerUserId 与当前登录账号关联展示。
设备验证：清空全部患者、管理 AI Key、导出快照、删除账号等敏感操作需通过 Face ID、Touch ID 或设备密码验证（LocalAuthentication）。
删除账号：将删除本机加密账号资料与密码摘要；不会自动删除已写入 SwiftData 的患者与查房记录，需您另行删除或卸载应用。
三、网络使用
本应用无开发者自有后端存储病历。以下网络通信均在您主动使用相关功能时发生：

3.1 互联网通信（HTTPS）
目标地址（默认）	用途	发送的数据
https://api.deepseek.com/v1/chat/completions	AI 问答、病程生成等（DeepSeek，用户可改 endpoint）	经 Safe Harbor 脱敏的提示词/问题；患者姓名默认替换为编号/床号；API Key 在请求头
https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions	同上（阿里云百炼）	同上
https://spark-api-open.xf-yun.com/v1/chat/completions	同上（讯飞星火）	同上
https://open.bigmodel.cn/api/paas/v4/chat/completions	同上（智谱 GLM）	同上
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi	医学文献检索（PubMed）	经脱敏的诊断检索词
https://www.ebi.ac.uk/europepmc/webservices/rest/search	医学文献检索（Europe PMC）	经脱敏的诊断检索词
AI 相关 HTTPS 请求采用 TLS 公钥 pinning（PinnedURLSessionFactory）。 第三方服务商对其收到的数据有各自隐私政策，请您在选用前审阅相应条款。

3.2 医患转写（录音转文书）
语音识别（ASR）：Release 正式版默认 apple_speech，使用 Apple 系统 Speech 框架在设备端处理音频，不上传至开发者服务器。
Whisper 本地 ASR：代码中预留 whisper_local 选项，但当前版本尚未集成 Core ML 模型；若误选将提示改用系统听写。正式版请勿启用该选项。
转写后 LLM 整理：若您在构建配置中设置 MedicalTranscriptLLMEndpoint / API Key，或通过「AI Key 管理」配置了聊天网关，医患转写流水线（MedicalTranscriptLLMClient）会将经脱敏的对话文本以 HTTPS POST 发送至您配置的 OpenAI 兼容 endpoint（默认与 AI 问答相同，如 DeepSeek / 百炼等）。开发者不提供中转存病历的云端服务。
3.3 增强本地 OCR 模型包（可选）
当前 Release 构建：EnhancedLocalOCRPackageManifestURL 为空，应用仅使用内置 OCR 模型包，不会为下载模型而访问互联网。
若定制构建配置了 manifest URL：您在「设置 → 病人录入 → 增强本地识别包」中主动下载时，应用将通过 HTTPS 访问该 manifest 地址及其指向的模型文件 URL，下载至本机并做 SHA256 校验；下载域名以您构建时填写的 URL 为准，启用前须在隐私政策与本节中更新具体域名。
3.4 局域网通信（HTTP）— SendToPC
「发送到电脑」功能（SendToPC）在您填写目标 IPv4 地址后，通过 HTTP GET 向同一局域网内端口 8888 的接收程序发送文本：

http://<您填写的IP>:8888/send?text=<脱敏文本>&token=…

发送前对正文执行与 AI 出站相同的脱敏处理（SensitiveTextRedactor）。
目标 IP 保存在本机 UserDefaults（targetPC_IP）。
该通道为明文 HTTP，仅限您可控的局域网环境；请确保接收端与网络安全策略符合您的机构要求。
电脑端接收程序：本应用不包含也不运营 PC 端接收服务。接收程序由您或所在机构自行部署与维护；其对收到文本的存储、日志、二次转发与合规责任由部署方自行承担，不在本隐私政策（开发者）范围内。使用前请确认机构信息安全规定。
3.5 本机 OCR 与图像
常规图像 OCR 使用 Apple Vision 框架在设备端处理，图像默认不上传云端 OCR 服务（与可选「增强本地 OCR 包」下载无关）。

3.6 未使用的网络能力
本应用未使用 WebSocket 连接 AI、远程推送（APNs）、蓝牙传输或 Bonjour 服务发现。

四、第三方服务与 SDK
本 iOS 工程未集成 CocoaPods / Swift Package Manager 第三方 SDK（无 Firebase、Crashlytics、广告 SDK、统计 SDK 等）。 运行时依赖 Apple 系统框架（SwiftUI、SwiftData、Vision、Speech、AVFoundation、LocalAuthentication、ActivityKit、UserNotifications、CryptoKit 等）。

您主动配置并调用的第三方在线服务包括：

DeepSeek — 大语言模型 API
阿里云百炼（DashScope） — 大语言模型 API
讯飞星火 — 大语言模型 API
智谱 GLM（BigModel） — 大语言模型 API
NCBI PubMed / Europe PMC — 公开医学文献检索
上述服务的 API Key 由您自行申请并仅存储于本机 Keychain。

附：App Store 能力与 Entitlements
本应用未启用以下 Apple 能力：远程推送（APNs）、iCloud/CloudKit 同步、HealthKit、App Groups、Sign in with Apple、Associated Domains、蓝牙后台。

已启用/使用的系统能力包括：

Live Activities（ActivityKit）：后台文书生成时在锁屏/灵动岛展示进度（仅床号）；扩展 Bundle ID：seayeh.SmartWardRound2.LiveActivity。
本地通知：医嘱未完成提醒（不含患者姓名）。
本地网络：SendToPC 局域网发送（见 3.4 节）。
Entitlements 配置文件见仓库 SmartWardRound2.entitlements 与 SmartWardRound2LiveActivity.entitlements；App Store Connect 中请勿勾选未使用的 Capability。详见仓库 docs/APP_STORE_CAPABILITIES.md。

五、系统权限
权限	用途
相机（NSCameraUsageDescription）	拍摄床头卡或腕带，识别床号与姓名并录入患者
相册（NSPhotoLibraryUsageDescription）	从相册选择检查报告等图片进行本地 OCR 导入
麦克风（NSMicrophoneUsageDescription）	查房语音转写、医患场景录音转写
语音识别（NSSpeechRecognitionUsageDescription）	将查房口述内容实时转为文字预览
Face ID / 设备密码（NSFaceIDUsageDescription）	清空患者、AI Key 管理、后台返回验证、删除账号等敏感操作前确认本人
本地网络（NSLocalNetworkUsageDescription）	将查房记录发送到同一热点下的内网电脑（SendToPC）
实时活动（Live Activities）	在锁屏/灵动岛展示文书生成或查房最小化状态（仅显示床号，不含姓名）
本地通知	当日 17:30 提醒未完成医嘱条数（通知正文不含患者姓名，仅显示数量）
本应用未请求：HealthKit、定位（CoreLocation）、蓝牙、通讯录、日历等权限。

六、患者数据与脱敏
本应用可存储真实患者标识信息（姓名、住院号/病历号、床号、年龄、诊断与病历正文等），仅供您在设备本地开展临床文书工作。
姓名永不上云策略：向 AI 发送的请求默认用编号/床号指代患者，并在 HTTP 发送前替换已知全名；住院号仍可能出现在提示中。
Safe Harbor 出站网关：对出站文本执行 HIPAA Safe Harbor 风格的脱敏（机构名、姓名、地理信息、日期、电话、邮箱、证件号、IP 等）。
OCR 脱敏流水线：检验/报告 OCR 结果在持久化前提供人工复核界面。
实时活动：文书生成进度在锁屏/灵动岛仅显示床号，不显示患者姓名。
本应用界面全局仅展示姓氏尊称（如「张先生」），不要求也不鼓励录入患者全名；录入时仅接受《百家姓》单姓或复姓。
本应用提供隐私遮蔽层与后台返回验证，用于降低日常窥屏风险，不构成医学合规或等保的完整证明。
七、数据安全
SwiftData 主库启用文件保护；临床敏感目录排除备份。
文书草稿与 PHI 侧车数据采用 AES-256-GCM 加密，密钥存于 Keychain。
AI API Key 仅存 Keychain，Compliance Checker 阻止 PHI 写入 UserDefaults。
AI HTTPS 通信采用 TLS 公钥 pinning。
越狱/篡改设备检测与代码签名校验可在启动时阻止临床功能使用。
复制临床正文时剪贴板设置较短有效期（默认约 2 分钟）；OCR 沙箱期间限制剪贴板访问。
八、数据删除与导出
您可通过以下方式管理数据：

导出加密 JSON 快照：设置 → 数据与安全 → 导出加密 JSON 快照（需口令，经系统分享离开本机）。
导出 PDF / Word：各文书页面的分享功能，文件先写入临时目录再由您选择去向。
删除单个患者：首页在院列表左滑删除。
清空全部患者：首页在院列表右上角（需 Face ID / 密码验证）。
清除缓存：设置 → 清除缓存（含离线 AI 队列、URL 缓存）。
清除加密草稿：设置 → 清除加密草稿（不删除 SwiftData 主库与账号资料）。
删除账号：设置 → 个人账号管理 → 删除账号与本地资料（需设备验证；不自动删除患者数据）。
卸载应用：将删除应用沙盒内全部数据。
向第三方 AI 服务商已发送的内容，本应用无法代为删除；请在相应服务商控制台按政策处理，并可在本应用轮换或清除 API Key。

九、儿童隐私
本应用面向具备执业资格或经授权使用电子病历系统的医疗专业人员，并非面向 13 周岁以下儿童设计，也不会故意收集儿童的个人信息。 若您发现儿童在未授权情况下使用了本应用并录入了信息，请通过下方联系方式与我们联系。

十、医疗免责
本应用为病历书写与查房辅助工具，不提供诊断、处方或急诊服务。 AI 生成内容仅供参考，不替代执业医师的临床判断。出现危急病情请立即就医。

十一、政策更新
我们可能随应用版本更新本政策。重大变更时，应用内隐私同意机制（当前版本号：2）可能要求您重新确认。 请定期查阅本页面或应用内「设置 → 数据与安全 → 隐私政策」。

十二、联系我们
如对本隐私政策或数据处理有疑问、投诉或行使相关权利，请联系：

开发者：叶海波
电子邮箱：m1568_mbtqqsozoz@aka.yeah.net
应用内亦可在「设置 → 关于与支持」查看上述联系方式与隐私政策链接。

智能查房（seayeh.SmartWardRound2）· 开发者：叶海波

生效日期：2026-06-11 · 最后更新：2026-06-11
