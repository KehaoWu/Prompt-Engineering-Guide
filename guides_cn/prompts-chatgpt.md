# ChatGPT提示工程

在本节中，我们介绍了 ChatGPT 的最新提示工程技术，包括技巧、应用、限制、论文和其他阅读资料。

**请注意，此部分尚未完成。** 

话题：
- [ChatGPT简介](#ChatGPT简介)
- [回顾对话任务](#回顾对话任务)
- [与ChatGPT的对话](#与ChatGPT的对话)
- [Python文档](#Python文档)

---

## ChatGPT简介

ChatGPT是一个由OpenAI训练的新模型，可以进行对话互动。该模型经过训练，遵循提示指令，以在对话上下文中提供适当的响应。ChatGPT可以帮助回答问题、建议食谱、以特定风格编写歌词、生成代码等等。

`ChatGPT` 是使用人类反馈强化学习训练的（`RLHF`）模型。虽然这个模型比以前的 `GPT` 迭代更加强大（而且还训练了以减少有害和不真实的输出为目标），但它仍然存在一些限制。让我们通过具体的例子来介绍一些能力和限制。

您可以在此处使用ChatGPT的研究预览[链接](https://chat.openai.com)，但对于下面的例子，我们将在OpenAI Playground上使用`Chat`模式。

---

## 回顾对话任务




在[之前的指南](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-basic-usage.md#conversation)中，我们简要介绍了对话功能和角色提示。我们介绍了如何指示LLM以特定风格、特定意图、行为和身份进行对话。




让我们复习一下我们以前的基本示例，我们创建了一个会话系统，能够生成更多技术性和科学性的回答来回答问题。




**提示：**
```



以下是与一位AI研究助手的对话。助手的口气是技术性和科学性。




Markdown格式：```人类：你好，你是谁？```

中文翻译：人类：你好，你是谁？


AI：您好！我是一名AI研究助手。今天我可以帮您做什么呢？


人类：你能告诉我关于黑洞的创造吗？


AI:
```





从上面的例子中，您可以看到两个重要的组件：


- 聊天机器人的 **意图** 或解释


- 指令聊天机器人回应时使用的风格或语气的 **身份**




上面的简单示例很适合使用使用 `text-davinci-003` 的文本补全 API。最近，OpenAI [宣布了ChatGPT API](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)，这是一个更强大、更便宜的模型，称为 `gpt-3.5-turbo`，专门为这种类型的功能（聊天补全）而构建。OpenAI 甚至推荐这个模型用于非聊天场景下的文本补全。使用 ChatGPT API 的其他好处包括显著降低成本（90％）和提高效率。




大型公司如 Snap Inc. 和 Instacart 已经在他们的产品上整合了由 ChatGPT 支持的对话功能，范围从个性化推荐到开放式购物目标。




---


## 与ChatGPT的对话




### 多回合对话




为了开始展示ChatGPT的功能，我们将使用上面的聊天机器人助手示例并讨论结果。与“text-davinci-003”相比，供动力ChatGPT的“gpt-3.5-turbo”模型使用聊天格式作为输入。该模型期望一系列消息作为输入，并使用这些消息生成回复。




**输入：**
```



系统：您是一名人工智能研究助手。请使用技术和科学的语气。


`USER：`你好，你是谁？


**助手：** 你好！我是一名人工智能研究助手。今天我能为您做些什么呢？


# 用户: 你能告诉我黑洞的创造吗？
```





**输出：**
```



助理：当然！黑洞是在一个巨大的恒星燃烧完燃料并在自身引力下坍塌时形成的。这种坍塌导致恒星变得极其密集，其引力场非常强，甚至连光都无法逃脱。这个点被称为事件视界，是不可逆的。一旦一个恒星越过事件视界，它被视为一个黑洞。
```





请注意，在上面的示例中，我简化了输入和输出，但是ChatGPT聊天完成API需要消息以特定格式呈现。下面是使用OpenAI Playground中的`Chat Mode`来查看此示例的快照：




请将下面的句子转换为 Markdown 格式，不发音：![](../img/chatgpt-1.png)




以下是我们示例的更正式的API调用示例：


请将下面的句子转换为Markdown格式的中文：```python


```python
import openai
```

请将上述Markdown格式的句子翻译成中文，无需发音：

```python
import openai
```




`openai.ChatCompletion.create()` 请翻译为中文（不发音）：


`model="gpt-3.5-turbo"` 的中文翻译为：`模型="gpt-3.5-turbo"`。


消息=[


{"角色": "系统", "内容": "您是一名人工智能研究助手。您使用技术和科学的语调。"}


{"角色": "用户", "内容": "你好，你是谁？"}


{"角色": "助手", "内容": "您好！我是一位AI研究助手。今天有什么能帮到您的吗？"}，


`{"role": "user", "content": "Can you tell me about the creation of black holes?"}` 的中文翻译: 

```
{"role": "用户", "content": "您能告诉我黑洞的形成吗?"}
```


请将下列句子使用 Markdown 格式翻译成中文：]


# 这是Markdown格式的句子。
```



未来开发者与ChatGPT的交互预计将通过[Chat Markup Language](https://github.com/openai/openai-python/blob/main/chatml.md)（简称为ChatML）进行。




### 单轮任务




对话格式可以进行多轮交互，但也支持类似于我们与`text-davinci-003`使用的单轮任务。这意味着我们可以使用ChatGPT执行类似于原始GPT模型所示的相似任务。例如，让我们尝试使用ChatGPT执行以下问答任务：




**输入：**
```



用户：根据以下语境回答问题。回答简短明了。如果不确定答案，请回答“不确定答案”。




上下文：Teplizumab的根源可以追溯到新泽西药品公司Ortho Pharmaceutical。在那里，科学家们生成了一种名为OKT3的抗体的早期版本。 最初从老鼠中获得，分子能够结合到T细胞表面并限制其细胞杀伤潜力。 1986年，它获得了批准，可帮助预防肾脏移植后的器官排斥，成为第一个允许人类使用的治疗性抗体。 

<!--上下文：Teplizumab的根源可以追溯到新泽西药品公司Ortho Pharmaceutical。在那里，科学家们生成了一种名为OKT3的抗体的早期版本。 最初从老鼠中获得，分子能够结合到T细胞表面并限制其细胞杀伤潜力。 1986年，它获得了批准，可帮助预防肾脏移植后的器官排斥，成为第一个允许人类使用的治疗性抗体。 -->




问题：OKT3最初来源于什么？




答案：
```





**输出：**
```



助手：老鼠。
```





请记住，我正在添加`USER`和`ASSISTANT`标签，以更好地演示如何使用ChatGPT执行任务。以下是在Playground中使用的示例：




请将以下句子使用Markdown格式翻译成中文，不发音: ![](../img/chatgpt-classic.png)




更正式地说，这是 API 调用（我仅包括请求的消息组件）：


把下方代码进行Markdown格式编写：```python


内容= """根据以下背景回答问题。回答要简短和简洁。如果不确定答案，请回答“不确定答案”。




# 上下文：特普利单抗源于新泽西州的药品公司Ortho Pharmaceutical。在那里，科学家们生成了一种名为OKT3的抗体的早期版本。最初来源于小鼠，该分子能够与T细胞表面结合并限制它们的杀伤细胞能力。1986年，它获得了批准，帮助预防肾移植后的器官排斥，成为第一种允许人类使用的治疗性抗体。




问题：OKT3最初是从哪里获取的？




回答：
Sorry, as an AI language model, I am unable to understand your request. Could you please provide me with more context or specific instructions so I can assist you better?




回应 = openai.ChatCompletion.create（）


`model="gpt-3.5-turbo"` 的Markdown格式的翻译为： `模型="gpt-3.5-turbo"`。


消息= [


{"role": "user", "content": 内容} ，请将其转换为Markdown格式的中文，不需要发音。


请将以下句子使用 markdown 格式翻译成中文而不发音： ],


温度=0，


请将以下句子使用 Markdown 格式翻译成中文：
```





### 指导聊天模型


根据官方OpenAI文档，`gpt-3.5-turbo`模型的快照也会被提供。例如，我们可以从3月1日的快照中访问`gpt-3.5-turbo-0301`。这允许开发人员选择特定的模型版本。这也意味着指导模型的最佳实践可能会随着版本的不同而改变。




当前的建议是将指令写在“用户”消息中，而不是可用的“系统”消息中，对于`gpt-3.5-turbo-0301`。




---

无需发音，请将下面的句子以Markdown格式翻译为中文：


## Python文档




|描述|笔记本|


|--|--| 的中文翻译是： |-—-|-—-|。


|了解如何使用 `openai` 库调用 ChatGPT API 的更多信息。|[ChatGPT API 介绍](../notebooks/pe-chatgpt-intro.ipynb)|


| 通过使用 `LangChain` 库来学习如何使用 ChatGPT 功能。 | [ChatGPT API with LangChain](../notebooks/pe-chatgpt-langchain.ipynb) |




---

Markdown是一种轻量级标记语言。


## 参考文献




- [Seeing ChatGPT Through Students' Eyes: An Analysis of TikTok Data](https://arxiv.org/abs/2303.05349) （2023年3月）


- [使用对话式语言模型和提示工程从研究论文中提取准确的材料数据-- ChatGPT的示例](https://arxiv.org/abs/2303.05352)（2023年3月）


- [ChatGPT 处于地平线上：大型语言模型可能是我们智能交通所需要的吗？](https://arxiv.org/abs/2303.05382)（2023 年 3 月）


- [制作计算机辩护律师](https://arxiv.org/abs/2303.05383) (2023年3月)


- [人工合成L液晶模块的数据是否有助于临床文本挖掘？](https://arxiv.org/abs/2303.04360)（2023年3月）


- [MenuCraft：具有大型语言模型的交互式菜单系统设计](https://arxiv.org/abs/2303.04496)（2023年3月）


- [AI 生成内容（AIGC）的综合调查：从 GAN 到 ChatGPT 的生成 AI 历史](https://arxiv.org/abs/2303.04226)（2023 年 3 月）


- [探索 ChatGPT 在事件抽取中的可行性](https://arxiv.org/abs/2303.03836)


- [ChatGPT：手动注释结束的开始？自动类型识别的应用案例](https://arxiv.org/abs/2303.03953) (2023年3月)


- [ChatGPT是一个好的NLG评估器吗？初步研究](https://arxiv.org/abs/2303.04048) (2023年3月)


- [Affective Computing是否将从基础模型和通用AI中出现？对ChatGPT的首次评估](https://arxiv.org/abs/2303.03186)（2023年3月）


- [UZH_CLyp在SemEval-2023 任务9中: 采用Head-First Fine-Tuning和 ChatGPT数据生成实现Tweet亲密度跨语言学习]（https://arxiv.org/abs/2303.01194）（2023年3月）


- [如何将输入格式化为ChatGPT模型](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb)（2023年3月）
- [ChatGPT能够评估人类个性吗？一般评估框架](https://arxiv.org/abs/2303.01248)（2023年3月）


- [通过ChatGPT进行跨语言摘要](https://arxiv.org/abs/2302.14229) (2023年2月)


-【ChatAug：利用ChatGPT进行文本数据增强】(https://arxiv.org/abs/2302.13007) （2023年2月）


- [Dr ChatGPT，请告诉我我想听的话：及时的知识如何影响健康答案的正确性](https://arxiv.org/abs/2302.13793) （2023年2月）


- [ChatGPT 在数学语文问题上的独立评估](https://arxiv.org/abs/2302.13814) (2023年2月)


- [ChatGPT: 2.5个月后的元分析](https://arxiv.org/abs/2302.13795)（2023年2月）


- [让我们聊聊吧！与 ChatGPT 的对话：技术、应用和局限性](https://arxiv.org/abs/2302.13817)（2023年2月）


- [检查你的事实，再试一次：通过外部知识和自动化反馈来改善大型语言模型](https://arxiv.org/abs/2302.12813) (2023年2月)


- [对ChatGPT的鲁棒性：对抗和超出分布的视角的研究](https://arxiv.org/abs/2302.12095) (2023年2月)


- [如何在SPC实践、教育和研究中（误）使用生成AI模型，如ChatGPT？一项探索性研究](https://arxiv.org/abs/2302.10916)（2023年2月）


- [ChatGPT是否也可以理解呢？ChatGPT和Fine-tuned BERT的比较研究](https://arxiv.org/abs/2302.10198)（2023年2月）


- [使用ChatGPT增强提示工程的提示模式目录](https://arxiv.org/abs/2302.11382) (2023年2月)


- [通过与ChatGPT聊天实现零样本信息提取](https://arxiv.org/abs/2302.10205)（2023年2月）


- [ChatGPT：万事通，无所精通](https://arxiv.org/abs/2302.10724)（2023年2月）


- [ChatGPT和DALL-E 2在决策和空间推理方面的初步评估](https://arxiv.org/abs/2302.09068) （2023年2月）


- [网民、学者和信息专业人士对具有特殊参考意义的聊天GPT的人工智能观点](https://arxiv.org/abs/2302.07136)（2023年2月）


- [ChatGPT 中的语言歧义分析](https://arxiv.org/abs/2302.06426) （2023 年 2 月）


- [ChatGPT与传统问答方式在知识图谱中的应用：当前状态及未来方向——迈向知识图谱聊天机器人](https://arxiv.org/abs/2302.06466)（2023年2月）


- [ChatGPT 和生成 AI 对科学意味着什么](https://www.nature.com/articles/d41586-023-00340-6) (2023 年 2 月)


- [使用BERT和ChatGPT对疫苗研究文献中的情感分析](https://arxiv.org/abs/2302.06474) (2023年2月)


- [\探索AI ChatGPT的伦理学：诊断分析\](https://arxiv.org/abs/2301.12867) （2023年1月）


- [ChatGPT for Good? 对大型语言模型在教育中的机遇和挑战的论述](https://www.edu.sot.tum.de/fileadmin/w00bed/hctl/_my_direct_uploads/ChatGPT_for_Good_.pdf) （2023 年 1 月）


- [会话型人工智能的政治意识形态：ChatGPT倾向于环保主义、左倾自由主义的收敛证据](https://arxiv.org/abs/2301.01768)（2023年1月）


- [提高可靠性的技巧 - OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/techniques_to_improve_reliability.md)


- [Awesome ChatGPT Prompts](https://github.com/f/awesome-chatgpt-prompts) 

（以Markdown格式翻译）


- [介绍ChatGPT](https://openai.com/blog/chatgpt) （2022年11月）




---

---

---
请使用Markdown格式将此句翻译成中文：


[前一节 (应用)](./prompts-applications.md)




[下一章节（对抗性提示）](./prompts-adversarial.md)