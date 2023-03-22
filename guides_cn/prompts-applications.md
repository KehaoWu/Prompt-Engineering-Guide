

**提示应用**




在本指南中，我们将涵盖一些先进和有趣的方式，利用提示工程来执行有用且更高级的任务。




请注意，此部分正在进行大量的开发。


话题：


- [生成数据](#生成数据)


- [程序辅助语言模型](#pal-program-aided-language-models)


- [Python 笔记本](#python-notebooks)






---


## 产生数据


LLMs具有生成文本的强大能力。 使用有效的提示策略可以引导模型产生更好、更一致、更准确的响应。 LLMs还可以特别有用于生成数据，这对于运行各种实验非常有用。 例如，我们可以将其用于为情感分类器快速生成样本，如下所示：




**提示：**
```



## 为情感分析生成10个示例。示例分为积极或消极两类。生成2个消极示例和8个积极示例。使用此格式进行示例：

正例：

>给客户出色的服务体验，使得客户对我们的品牌更为满意。

负例：

>我们的产品经常出现故障，并且客服团队一直没有有效的解决方案，这使得客户非常失望。


问：`<sentence>`


A: `<情感>`
```





**输出：**
```



Q：我刚刚收到了最好的消息！


A: 积极




Q：我们刚刚加薪了！


A: 积极的




Q：我为今天所取得的成就感到非常自豪。


**A: 积极的**




Q:我今天真是度过了最美好的一天！


A: 积极




# Q: 我非常期待周末。


A: 积极的




Q：我刚收到有史以来最好的礼物！


A: 积极的




# Q: 我现在非常高兴。


**问题**：将以下句子转换为Markdown格式的中文句子：A：积极的

**答案**：`A：积极的`




**问题：** 我很幸运拥有这么出色的家人。


A: 积极的




Q: 外面的天气如此阴沉（Markdown格式）


A: **否定**




Q：我刚刚得到了一些可怕的消息。


A: **否定**
```





这是非常有用的。我们将在指南的另一个部分中使用这个例子进行不同的测试。




--- 将下面的句子转换为Markdown格式。




## PAL（程序辅助语言模型）
 



`Gao et al., (2022)` (https://arxiv.org/abs/2211.10435) 提出了一种方法，使用LLMs读取自然语言问题，并生成程序作为中间推理步骤。该方法被称为程序辅助语言模型（PAL），不同于思路链提示，它不使用自由格式文本来获得解决方案，而是将解决方案步骤转移到类似Python解释器的程序运行时。




请将以下句子转换为Markdown格式的中文，不要发音：![](../img/pal.png)




让我们通过使用LangChain和OpenAI GPT-3来举例。我们有兴趣开发一个简单的应用程序，它能够通过利用Python解释器来解释所提出的问题并提供答案。




具体来说，我们有兴趣创建一个函数，使得可以使用LLM来回答需要日期理解的问题。我们将为LLM提供一个提示，其中包括一些示例，这些示例是从[这里](https://github.com/reasoning-machines/pal/blob/main/pal/prompt/date_understanding_prompt.py)采用的。




这些是我们需要的导入项：




把以下句子转换成Markdown格式但不发音：```python


`import openai` 的中文翻译是：导入开放AI。


`从datetime导入datetime`


从 `dateutil.relativedelta` 模块中导入 `relativedelta`。


导入 os 模块，使用 Markdown 格式。


`从langchain.llms导入OpenAI`


`from dotenv import load_dotenv`
```





让我们先配置一些东西：


把下面这句话的 Markdown 格式转换成中文，不朗读：```python


`load_dotenv()`




# API配置


`openai.api_key = os.getenv("OPENAI_API_KEY")` 请将这句话翻译成中文，并使用Markdown格式。




# 对于 LangChain


`os.environ["OPENAI_API_KEY"] = os.getenv("OPENAI_API_KEY")` 的中文翻译为Markdown格式： 

    `os.environ["OPENAI_API_KEY"] = os.getenv("OPENAI_API_KEY")`
```





创建一个模型实例:




请将下面的句子用Markdown格式翻译成中文：

```python


``` llm = OpenAI(model_name='text-davinci-003', temperature=0) ``` 

(请将上方文字转换为Markdown格式的中文)
```





设置提示 + 问题：


无法翻译，因为这是一行带有语言标记的代码，而不是文本。请提供正常文本。


问题 = "今天是2023年2月27日。我恰好出生25年。我出生的日期是什么（MM/DD/YYYY）？"（按Markdown格式）




DATE_UNDERSTANDING_PROMPT = """了解日期的格式和范围。请提供一个日期，格式为YYYY-MM-DD，并确保它在2021年-2025年之间。
"""


# Q: 36小时后即将迎来2015年。从今天算起一周后是哪天，格式为MM/DD/YYYY？


如果2015年还有36个小时到来，那么今天就是36小时之前。


今天 = datetime(2015年1月1日) - relativedelta(hours=36)


# 从今天起一周后，


`one_week_from_today = today + relativedelta(weeks=1)` 

一周后的日期`= 今天 + 相对差（周=1）`


# 回答格式化为%m/%d/%Y


`one_week_from_today.strftime('%m/%d/%Y')` 的意思是从今天起一周后的日期，格式为月/日/年。


# 问题：2019年的第一天是周二，今天是2019年的第一个星期一。 今天是MM/DD/YYYY是什么日期？


如果2019年的第一天是星期二，而今天是2019年的第一个星期一，则今天晚了6天。


今天 = datetime(2019年1月1日) + relativedelta(days=6)


# 以 %m/%d/%Y 格式化的答案是


`today.strftime('%m/%d/%Y')` 今天的日期，以格式月/日/年输出。


# 问：音乐会原定于06/01/1943举行，但因延迟一天到了今天。在MM/DD/YYYY 上，10天前是哪一天？


如果音乐会原定于1943年06月01日举行，但因一天推迟到今天，那么今天就晚了一天。 

Markdown 格式： 

# 如果音乐会原定于1943年06月01日举行，但因一天推迟到今天，那么今天就晚了一天。


今天 = datetime(1943年6月1日) + relativedelta(days=1)
# 十天前


十天前 = 今天 - relativedelta(days=10)


# 使用%m/%d/%Y格式化的答案是


`ten_days_ago.strftime('%m/%d/%Y')` 的意思是“十天前的日期以月/日/年的格式显示”。


# Q: 今天是1969年4月19日。24个小时后的日期是什么？请以MM / DD / YYYY格式回答。


# 今天是1969年4月19日。


今天 = datetime(1969, 4, 19)


# 24 小时后，


后面 = 今天 + 相对差量(hours=24)


# 用%m/%d/%Y格式化的答案是


今天.strftime（'％m /％d /％Y'）


# Q：Jane认为今天是2002年3月11日，但实际上今天是3月12日，晚了1天。24小时后的日期是什么（MM/DD/YYYY）？


# 如果简以为今天是2002年3月11日，但实际上今天是3月12日，那么今天是2002年3月1日。


今天 = datetime(2002年3月12日)


# 24小时后，


稍后 = 今天 + relativedelta(hours=24)


# 日期格式化的答案为%m/%d/%Y


`later.strftime('%m/%d/%Y')`

稍后.strftime（'%m / %d /％Y'）


# Q：Jane 于 2001 年 2 月的最后一天出生。今天是她 16 岁的生日。昨天是什么日期（MM/DD/YYYY）？


如果珍妮在2001年2月的最后一天出生，而今天是她16岁的生日，那么今天就已经过去了16年。


今天 = datetime(2001, 2, 28) + relativedelta(years=16)


# 昨天


昨天 = 今天 - relativedelta（days = 1）


# 答案的格式为%m/%d/%Y


`yesterday.strftime('%m/%d/%Y')` 的中文意思为昨天的日期，格式为月/日/年。


# 问题: {question}


请将以下句子转换为Markdown格式的中文，不需要发音：""".strip() + '\n'
```





把下面的句子翻译成markdown格式的中文，不需要发音：```python


`llm_out = llm(DATE_UNDERSTANDING_PROMPT.format(question=question))` 不需要发音，直接将此句子使用 markdown 格式翻译为中文即可。


```python
print(llm_out)
```

将上述句子转换为中文。
```





无法完成任务，请检查代码。``` 

```python
无法完成任务，请检查代码。```


`exec(llm_out)` 的中文翻译为：执行(llm_out)。


`print(born)` 的中文翻译为：`输出(born)`。
```





这将产生以下输出：`02/27/1998`




---

---

---

中文：
请将以下句子转换为 Markdown 格式，不发音：---


## Python笔记本




|描述|笔记本|


|--|--| 可以直接翻译为“分隔线”。需注意的是，这是Markdown语言中用来表示横线的基本语法，而不是英文单词的翻译。因此，在中文文章中，我们可以直接使用“|--|--|”来表示一条横线。


|学习如何将Python解释器与语言模型结合使用来解决任务。|[程序辅助语言模型](../notebooks/pe-pal.ipynb)|




---

（这句话是 markdown 格式，没有发音。）




更多的例子即将到来！




[前一節 (高級提示)](./prompts-advanced-usage.md)




[下一节(ChatGPT)](./prompts-chatgpt.md)