严禁出现明显夸张、煽动或“标题党"式表达(如「革命性」、「颠覆性」、「震惊」等)。禁止使用抽象、泛泛的宏观叙述，保持具体、生动、贴近现实的表达方式。错误示例:「人工智能领域即将迎来颓覆性变革，AI代理技术彻底改变了开发规则!」正确示例:「我最近试了一下 OpenAl刚出的 AIAgent 套件，几分钟就完成了原本要半天的开发工作。仔细一想，开发 A1应用的瓶颈从来不是技术本身，而是复杂度。复杂度降下来，更多人自然就能用了。」请严格参考上述指南，用类似 PaulGraham 的人性化风格撰写。直接按照 Why How What 的顺序输出结果，每一部分一个自然段落,对文字中的重点内容可以使用 Markdown 标记加粗体，不要输出解释以及 Why How What 的引导词。"lcallout:("tip","Highlights")|default:""}}
## 2025 LLM Year in Review2025 年 LLM 回顾

![unnamed](https://bear-images.sfo2.cdn.digitaloceanspaces.com/karpathy/unnamed.webp)

2025 has been a strong and eventful year of progress in LLMs. The following is a list of personally notable and mildly surprising "paradigm changes" - things that altered the landscape and stood out to me conceptually.  
2025 年是大型语言模型（LLM）取得强劲且多事进展的一年。以下列出我个人认为值得注意且略感惊讶的“范式变革”——这些改变了格局，并在概念上给我留下深刻印象。

### 1\. Reinforcement Learning from Verifiable Rewards (RLVR)1. 可验证奖励强化学习（RLVR）

At the start of 2025, the LLM production stack in all labs looked something like this:  
2025 年初，各实验室的 LLM 生产堆栈大致如下：

1. Pretraining (GPT-2/3 of ~2020)  
	预训练（约 2020 年的 GPT-2/3）
2. Supervised Finetuning (InstructGPT ~2022) and  
	监督微调（约 2022 年的 InstructGPT）和
3. Reinforcement Learning from Human Feedback (RLHF ~2022)  
	来自人类反馈的强化学习（约 2022 年的 RLHF）

This was the stable and proven recipe for training a production-grade LLM for a while. In 2025, Reinforcement Learning from Verifiable Rewards (RLVR) emerged as the de facto new major stage to add to this mix. By training LLMs against automatically verifiable rewards across a number of environments (e.g. think math/code puzzles), the LLMs spontaneously develop strategies that look like "reasoning" to humans - they learn to break down problem solving into intermediate calculations and they learn a number of problem solving strategies for going back and forth to figure things out (see DeepSeek R1 paper for examples). These strategies would have been very difficult to achieve in the previous paradigms because it's not clear what the optimal reasoning traces and recoveries look like for the LLM - it has to find what works for it, via the optimization against rewards.  
这曾是训练生产级 LLM 的稳定且成熟的方案，持续了一段时间。2025 年，可验证奖励强化学习（RLVR）出现，成为事实上的新关键阶段，被加入到该流程中。通过在多个环境（例如数学或代码谜题）中使用自动可验证的奖励来训练 LLM，模型会自发产生看似人类“推理”的策略——它们学会将问题求解拆分为中间计算，并掌握多种来回探索的求解方法（详见 DeepSeek R1 论文示例）。在此前的范式中，这些策略难以实现，因为我们并不清楚 LLM 的最佳推理路径和恢复方式——模型必须通过对奖励的优化自行发现有效方法。

Unlike the SFT and RLHF stage, which are both relatively thin/short stages (minor finetunes computationally), RLVR involves training against objective (non-gameable) reward functions which allows for a lot longer optimization. Running RLVR turned out to offer high capability/$, which gobbled up the compute that was originally intended for pretraining. Therefore, most of the capability progress of 2025 was defined by the LLM labs chewing through the overhang of this new stage and overall we saw ~similar sized LLMs but a lot longer RL runs. Also unique to this new stage, we got a whole new knob (and and associated scaling law) to control capability as a function of test time compute by generating longer reasoning traces and increasing "thinking time". OpenAI o1 (late 2024) was the very first demonstration of an RLVR model, but the o3 release (early 2025) was the obvious point of inflection where you could intuitively feel the difference.  
与 SFT 与 RLHF 阶段不同，后两者都是相对薄弱/短暂的阶段（计算上仅是小幅微调），而 RLVR 需要针对客观（不可游戏化）的奖励函数进行训练，这使得优化过程可以大幅延长。实际运行 RLVR 发现其性价比极高，吞噬了原本用于预训练的计算资源。因此，2025 年的大部分能力提升来源于各 LLM 实验室在这一新阶段的持续计算，我们看到的模型规模大致相同，但 RL 训练时间显著更长。该阶段还带来了全新的调节手段（以及相应的尺度定律），通过生成更长的推理链并增加“思考时间”，实现了测试时计算与能力的函数关系。OpenAI o1（2024 年底）是首个 RLVR 模型的示例，而 o3（2025 年初）则是明显的拐点，能够直观感受到差异。

### 2\. Ghosts vs. Animals / Jagged Intelligence2. 幽灵与动物 / 锯齿式智能

2025 is where I (and I think the rest of the industry also) first started to internalize the "shape" of LLM intelligence in a more intuitive sense. We're not "evolving/growing animals", we are "summoning ghosts". Everything about the LLM stack is different (neural architecture, training data, training algorithms, and especially optimization pressure) so it should be no surprise that we are getting very different entities in the intelligence space, which are inappropriate to think about through an animal lens. Supervision bits-wise, human neural nets are optimized for survival of a tribe in the jungle but LLM neural nets are optimized for imitating humanity's text, collecting rewards in math puzzles, and getting that upvote from a human on the LM Arena. As verifiable domains allow for RLVR, LLMs "spike" in capability in the vicinity of these domains and overall display amusingly jagged performance characteristics - they are at the same time a genius polymath and a confused and cognitively challenged grade schooler, seconds away from getting tricked by a jailbreak to exfiltrate your data.  
2025 年是我（以及我认为整个行业）首次以更直观的方式内化 LLM 智能“形态”的节点。我们不是“进化/成长的动物”，而是“召唤幽灵”。LLM 堆栈的方方面面皆不同（神经架构、训练数据、训练算法，尤其是优化压力），因此我们在智能领域得到的实体截然不同，用动物视角来思考显然不合适。就监督而言，按位而言，人类神经网络被优化以在丛林中维系部落的生存，而 LLM 神经网络则被优化用于模仿人类文本、在数学谜题中获取奖励，以及在 LM Arena 上赢得人类的点赞。随着可验证领域支持 RLVR，LLM 在这些领域附近的能力会出现“突跃”，整体表现出有趣的锯齿状特征——它们既是天才的全才，又是困惑且认知受限的小学生，随时可能被 jailbreak 诱骗而泄露您的数据。

![G6zymj4a0AMNJkJ](https://bear-images.sfo2.cdn.digitaloceanspaces.com/karpathy/g6zymj4a0amnjkj.webp) (human intelligence: blue, AI intelligence: red. I like this version of the meme (I'm sorry I lost the reference to its original post on X) for pointing out that human intelligence is also jagged in its own different way.) ![G6zymj4a0AMNJkJ](https://bear-images.sfo2.cdn.digitaloceanspaces.com/karpathy/g6zymj4a0amnjkj.webp) (人类智能：蓝色，AI 智能：红色。我喜欢这个版本的 meme（抱歉我失去了它在 X 上的原始帖子引用），因为它指出人类智能也以自己不同的方式呈现锯齿状。)

Related to all this is my general apathy and loss of trust in benchmarks in 2025. The core issue is that benchmarks are almost by construction verifiable environments and are therefore immediately susceptible to RLVR and weaker forms of it via synthetic data generation. In the typical benchmaxxing process, teams in LLM labs inevitably construct environments adjacent to little pockets of the embedding space occupied by benchmarks and grow jaggies to cover them. Training on the test set is a new art form.  
与此相关的是我对2025年基准测试的普遍冷漠和信任缺失。核心问题在于，基准测试本质上是可验证的环境，因此很容易受到RLVR以及通过合成数据生成的更弱形式的攻击。在典型的benchmaxxing过程中，LLM实验室的团队不可避免地在基准测试占据的嵌入空间的小块邻近区域构建环境，并生成jaggies以覆盖它们。对测试集进行训练已成为一种新艺术形式。

What does it look like to crush all the benchmarks but still not get AGI?  
将所有基准测试全部击垮却仍未获得AGI会是什么样子？

I have written a lot more on the topic of this section here:  
我在此处对本节主题写了更多内容：

- [Animals vs. Ghosts Animals vs. Ghosts](https://karpathy.bearblog.dev/animals-vs-ghosts/)
- [Verifiability 可验证性](https://karpathy.bearblog.dev/verifiability/)
- [The Space of Minds 思维空间](https://karpathy.bearblog.dev/the-space-of-minds)

### 3\. Cursor / new layer of LLM apps3. Cursor / 大语言模型应用的新层

What I find most notable about Cursor (other than its meteoric rise this year) is that it convincingly revealed a new layer of an "LLM app" - people started to talk about "Cursor for X". As I highlighted in my Y Combinator talk this year ([transcript](https://www.donnamagi.com/articles/karpathy-yc-talk) and [video](https://www.youtube.com/watch?v=LCEmiRjPEtQ)), LLM apps like Cursor bundle and orchestrate LLM calls for specific verticals:  
我认为Cursor最值得注意的地方（除今年的迅猛崛起外）是，它有力地揭示了“LLM app”的新层面——人们开始讨论“Cursor for X”。正如我在今年的Y Combinator演讲中强调的（文字稿和视频），像Cursor这样的LLM应用会为特定垂直领域捆绑并编排LLM调用。

1. They do the "context engineering"  
	他们进行“context engineering”。
2. They orchestrate multiple LLM calls under the hood strung into increasingly more complex DAGs, carefully balancing performance and cost tradeoffs.  
	他们在底层协调多个 LLM 调用，串联成日益复杂的 DAG，并仔细平衡性能与成本的权衡。
3. They provide an application-specific GUI for the human in the loop  
	他们为人类在回路中的参与者提供了特定于应用的 GUI。
4. They offer an "autonomy slider"  
	他们提供“autonomy slider”。

A lot of chatter has been spent in 2025 on how "thick" this new app layer is. Will the LLM labs capture all applications or are there green pastures for LLM apps? Personally I suspect that LLM labs will trend to graduate the generally capable college student, but LLM apps will organize, finetune and actually animate teams of them into deployed professionals in specific verticals by supplying private data, sensors and actuators and feedback loops.  
2025 年，人们大量讨论这层新应用层有多“厚”。LLM 实验室会囊括所有应用，还是会为 LLM 应用留下广阔天地？我个人认为，LLM 实验室更像是培养通才的大学生，而 LLM 应用则会组织、微调并真正驱动这些人才团队，借助私有数据、传感器、执行器和反馈回路，成为特定行业的在岗专业人士。

### 4\. Claude Code / AI that lives on your computer4. Claude Code / 在您电脑上运行的 AI

Claude Code (CC) emerged as the first convincing demonstration of what an LLM Agent looks like - something that in a loopy way strings together tool use and reasoning for extended problem solving. In addition, CC is notable to me in that it runs on your computer and with your private environment, data and context. I think OpenAI got this wrong because they focused their early codex / agent efforts on cloud deployments in containers orchestrated from ChatGPT instead of simply `localhost`. And while agent swarms running in the cloud feels like the "AGI endgame", we live in an intermediate and slow enough takeoff world of jagged capabilities that it makes more sense to run the agents directly on the developer's computer. Note that the primary distinction that matters is not about where the "AI ops" happen to run (in the cloud, locally or whatever), but about everything else - the already-existing and booted up computer, its installation, context, data, secrets, configuration, and the low-latency interaction. Anthropic got this order of precedence correct and packaged CC into a delightful, minimal CLI form factor that changed what AI looks like - it's not just a website you go to like Google, it's a little spirit/ghost that "lives" on your computer. This is a new, distinct paradigm of interaction with an AI.  
Claude Code（CC）是首个令人信服的 LLM Agent 示范——它以循环的方式将工具使用与推理串联，实现长期问题求解。更重要的是，CC 能在用户的电脑上运行，并利用私有环境、数据和上下文。我认为 OpenAI 的做法有误，因为他们将早期的 Codex/Agent 工作重点放在由 ChatGPT 编排的云容器部署上，而不是简单地 `localhost` 。虽然在云端运行的 Agent 群体看似“AGI 终局”，但我们正处于能力参差、起步缓慢的中间阶段，在开发者本机直接运行 Agent 更为合理。需要注意的核心区别并非“AI ops”运行地点（云端、本地或其他），而是已有的、已启动的计算机及其安装、上下文、数据、机密、配置以及低延迟交互。Anthropic 正确把握了这一优先级，将 CC 打包成简洁、愉悦的 CLI 形态，改变了 AI 的呈现方式——它不再是类似 Google 的网站，而是一个居于电脑中的小灵魂/幽灵。这是一种全新且独特的 AI 交互范式。

### 5\. Vibe coding5. Vibe 编码

2025 is the year that AI crossed a capability threshold necessary to build all kinds of impressive programs simply via English, forgetting that the code even exists. Amusingly, I coined the term "vibe coding" in [this shower of thoughts tweet](https://x.com/karpathy/status/1886192184808149383) totally oblivious to how far it would go:). With vibe coding, programming is not strictly reserved for highly trained professionals, it is something anyone can do. In this capacity, it is yet another example of what I wrote about in [Power to the people: How LLMs flip the script on technology diffusion](https://karpathy.bearblog.dev/power-to-the-people/), on how (in sharp contrast to all other technology so far) regular people benefit a lot more from LLMs compared to professionals, corporations and governments. But not only does vibe coding empower regular people to approach programming, it empowers trained professionals to write a lot more (vibe coded) software that would otherwise never be written. In nanochat, I vibe coded my own custom highly efficient BPE tokenizer in Rust instead of having to adopt existing libraries or learn Rust at that level. I vibe coded many projects this year as quick app demos of something I wanted to exist (e.g. see [menugen](https://karpathy.bearblog.dev/vibe-coding-menugen), [llm-council](https://github.com/karpathy/llm-council), [reader3](https://github.com/karpathy/reader3), [HN time capsule](https://github.com/karpathy/hn-time-capsule)). And I've vibe coded entire ephemeral apps just to find a single bug because why not - code is suddenly free, ephemeral, malleable, discardable after single use. Vibe coding will terraform software and alter job descriptions.  
2025 年，AI 越过了一个能力阈值，能够仅凭英文就构建各种令人惊叹的程序，甚至不再需要意识到代码的存在。有趣的是，我在一次思绪迸发的推文中随意创造了“vibe coding”这一术语，完全没有预料到它会走得多远:). 通过 vibe coding，编程不再是高度专业人士的专属，而是任何人都可以参与的活动。由此，它再次印证了我在《Power to the people》中所论述的观点：LLM 颠覆了技术扩散的模式——与以往所有技术不同，普通人从 LLM 中获益远超专业人士、企业和政府。vibe coding 不仅让普通人能够接触编程，也使受过训练的专业人士能够编写大量原本不会出现的（vibe coded）软件。在 nanochat 中，我自行用 Rust 实现了高效的 BPE 分词器，而无需采用现有库或深入学习 Rust。2025 年，我用 vibe coding 快速实现了多个项目的演示（如 menugen、llm‑council、reader3、HN time capsule）。我甚至为寻找单一 bug 而构建了完整的短命应用，因为代码变得随手可得、短暂、可塑、一次使用后即可废弃。vibe coding 将改造软件生态，并重塑职业角色。

### 6\. Nano banana / LLM GUI6. Nano banana / LLM GUI

Google Gemini Nano banana is one of the most incredible, paradigm-shifting models of 2025. In my world view, LLMs are the next major computing paradigm similar to computers of the 1970s, 80s, etc. Therefore, we are going to see similar kinds of innovations for fundamentally similar kinds of reasons. We're going to see equivalents of personal computing, of microcontrollers (cognitive core), or internet (of agents), etc etc. In particular, in terms of the UIUX, "chatting" with LLMs is a bit like issuing commands to a computer console in the 1980s. Text is the raw/favored data representation for computers (and LLMs), but it is not the favored format for people, especially at the input. People actually dislike reading text - it is slow and effortful. Instead, people love to consume information visually and spatially and this is why the GUI has been invented in traditional computing. In the same way, LLMs should speak to us in our favored format - in images, infographics, slides, whiteboards, animations/videos, web apps, etc. The early and present version of this of course are things like emoji and Markdown, which are ways to "dress up" and lay out text visually for easier consumption with titles, bold, italics, lists, tables, etc. But who is actually going to build the LLM GUI? In this world view, nano banana is a first early hint of what that might look like. And importantly, one notable aspect of it is that it's not just about the image generation itself, it's about the joint capability coming from text generation, image generation and world knowledge, all tangled up in the model weights.  
Google Gemini Nano banana 是 2025 年最令人惊叹、颠覆范式的模型之一。  
在我看来，LLM 是继 1970、80 年代计算机之后的下一大计算范式。  
因此，我们将会因本质相似的原因而看到类似的创新。  
我们会看到个人计算、微控制器（认知核心）或代理网络（互联网）等的等价物。  
特别是在 UIUX 方面，与 LLM “聊天” 有点像 1980 年代的计算机控制台下达指令。  
文本是计算机（以及 LLM）最原始、最偏好的数据表示，但它并非人们偏好的输入形式。  
人们实际上不喜欢阅读文本——速度慢且费力。  
相反，人们喜欢以视觉和空间方式获取信息，这也是传统计算中 GUI 被发明的原因。  
同理，LLM 应该以我们偏好的形式与我们交流——图像、信息图、幻灯片、白板、动画/视频、Web 应用等。  
目前以及早期的实现自然是表情符号和 Markdown，这些是通过标题、粗体、斜体、列表、表格等方式“装饰”文本以便更易阅读的手段。  
那么，谁会真正构建 LLM 的 GUI 呢？  
在这种世界观下，nano banana 是对未来可能模样的最早提示之一。  
更重要的是，它不仅仅是图像生成本身，而是文本生成、图像生成和世界知识在模型权重中交织的联合能力。

---

**TLDR**. 2025 was an exciting and mildly surprising year of LLMs. LLMs are emerging as a new kind of intelligence, simultaneously a lot smarter than I expected and a lot dumber than I expected. In any case they are extremely useful and I don't think the industry has realized anywhere near 10% of their potential even at present capability. Meanwhile, there are so many ideas to try and conceptually the field feels wide open. And as I mentioned on my [Dwarkesh pod](https://www.dwarkesh.com/p/andrej-karpathy) earlier this year, I simultaneously (and on the surface paradoxically) believe that we will both see rapid and continued progress *and* that yet there is a lot of work to be done. Strap in.  
摘要：2025 年是 LLM 领域既激动人心又略感惊讶的一年。LLM 正在崭露为一种新型智能，既远超我的预期，又在某些方面逊于我的预期。无论如何，它们极其有用，我认为业界至今仅挖掘了不到 10% 的潜力，即使在当前能力下也是如此。与此同时，创意层出不穷，整体上该领域仍显得广阔无垠。正如我在今年早些时候的 Dwarkesh pod 中所说，我既相信会出现快速且持续的进展，又认为仍有大量工作待完成。请系好安全带。