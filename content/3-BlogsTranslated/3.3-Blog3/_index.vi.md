---
title: "Giao thức mở cho khả năng tương tác của Agent, Phần 3: Strands Agents & MCP"
date: 2025-07-10
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Giao thức mở cho khả năng tương tác của Agent, Phần 3: Strands Agents & MCP

**Bởi Nick Aldridge, James Ward, và Clare Liguori | 10 THÁNG 7 NĂM 2025**

**Danh mục:** Trí tuệ Nhân tạo, Giải pháp Khách hàng, Mã nguồn Mở

---

## Giới thiệu

Các nhà phát triển đang kiến trúc và xây dựng các hệ thống gồm các AI agent có khả năng làm việc cùng nhau để tự động hoàn thành nhiệm vụ của người dùng. Trong [Phần 1](https://aws.amazon.com/blogs/opensource/open-protocols-for-agent-interoperability-part-1-inter-agent-communication-on-mcp/) của loạt bài blog về Giao thức mở cho khả năng tương tác của Agent, chúng tôi đã đề cập đến cách Giao thức Ngữ cảnh Mô hình (MCP) có thể được sử dụng để tạo điều kiện giao tiếp giữa các agent và các cải tiến đặc tả MCP mà AWS đang thực hiện để hiện thực hóa điều đó. Các ví dụ trong Phần 1 đã sử dụng Spring AI và Java để xây dựng các agent và kết nối chúng với MCP. Trong [Phần 2](https://aws.amazon.com/blogs/opensource/open-protocols-for-agent-interoperability-part-2-authentication-on-mcp/) chúng tôi đã thảo luận về Xác thực trên MCP. Đây là một khía cạnh quan trọng của việc kết nối các agent để chúng có thể làm việc cùng nhau trong một hệ thống lớn hơn, tất cả đều có kiến thức về người dùng là ai.

Trong Phần 3, chúng tôi sẽ trình bày cách bạn có thể xây dựng các hệ thống liên agent với [Strands Agents SDK](https://strandsagents.com/) mới và MCP.

Strands Agents là một SDK mã nguồn mở áp dụng phương pháp tiếp cận dựa trên mô hình để xây dựng và chạy các AI agent chỉ với vài dòng mã Python. Bạn có thể đọc thêm về Strands Agents trong [tài liệu](https://strandsagents.com/). Vì Strands Agents hỗ trợ MCP, chúng ta có thể nhanh chóng xây dựng một hệ thống bao gồm nhiều agent kết nối với nhau và sau đó triển khai các agent đó trên AWS.

Ví dụ của chúng ta là một agent Nhân sự (HR agent) có thể trả lời các câu hỏi về nhân viên. Để làm được điều này, bạn có thể hình dung agent Nhân sự sẽ giao tiếp với một số agent khác như agent dữ liệu nhân viên, agent Hoạch định Nguồn lực Doanh nghiệp (ERP), agent hiệu suất, agent mục tiêu, v.v. Đối với ví dụ này, hãy bắt đầu với một kiến trúc cơ bản trong đó một REST API cung cấp quyền truy cập vào một agent Nhân sự, agent này sẽ kết nối với agent thông tin nhân viên:

![Kiến trúc ví dụ HR agent](https://d2908q01vomqb2.cloudfront.net/ca3512f4dfa95a03169c5a670a4c91a19b3077b4/2025/07/10/HR-agent-architecture.png)

**Kiến trúc ví dụ HR Agent**

> **Lưu ý:** Phiên bản đầy đủ và có thể chạy được của ví dụ dưới đây có sẵn trong [repo mẫu Agentic AI](https://github.com/aws-samples/sample-agentic-ai-demos/tree/main/modules/strands-mcp-inter-agent).

---

## Tạo Hệ Thống Các Agent với Strands Agents và MCP

Hãy bắt đầu với MCP server sẽ cung cấp dữ liệu nhân viên để sử dụng trong agent thông tin nhân viên. Đây là một MCP server cơ bản:

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("employee-server", stateless_http=True, host="0.0.0.0", port=8002)

@mcp.tool()
def get_skills() -> set[str]:
    """all of the skills that employees may have - use this list to figure out related skills"""
    return SKILLS

@mcp.tool()
def get_employees_with_skill(skill: str) -> list[dict]:
    """employees that have a specified skill - output includes fullname (First Last) and their skills"""
    skill_lower = skill.lower()
    return [employee for employee in EMPLOYEES if any(s.lower() == skill_lower for s in employee["skills"])]

if __name__ == "__main__":
    mcp.run(transport="streamable-http")
```

MCP server này cung cấp khả năng lấy danh sách kỹ năng nhân viên và lấy danh sách các nhân viên có kỹ năng cụ thể. Đối với ví dụ này, chúng tôi đang sử dụng dữ liệu giả định và trong blog tương lai, chúng tôi sẽ chia sẻ cách bảo mật dịch vụ này.

Bây giờ chúng ta đã có cách để agent lấy thông tin nhân viên này, hãy tạo employee agent sử dụng Strands Agents. Agent của chúng ta kết nối với MCP server để lấy thông tin nhân viên và sử dụng Bedrock để suy luận. Chúng ta cũng thêm một system prompt để cung cấp thêm một số hành vi cho agent này:

```python
EMPLOYEE_INFO_URL = os.environ.get("EMPLOYEE_INFO_URL", "http://localhost:8002/mcp/")
employee_mcp_client = MCPClient(lambda: streamablehttp_client(EMPLOYEE_INFO_URL))

bedrock_model = BedrockModel(
    model_id="amazon.nova-micro-v1:0",
    region_name="us-east-1",
)

def employee_agent(question: str):
    with employee_mcp_client:
        tools = employee_mcp_client.list_tools_sync()

        agent = Agent(model=bedrock_model, tools=tools, system_prompt="you must abbreviate employee first names and list all their skills", callback_handler=None)

        return agent(question)
```

Ví dụ này sử dụng Amazon Bedrock và mô hình Nova Micro với công cụ dữ liệu nhân viên để suy luận đa lượt. Suy luận đa lượt là khi một AI agent thực hiện nhiều lần gọi đến một mô hình AI trong một vòng lặp, thường liên quan đến việc gọi các công cụ để lấy hoặc cập nhật dữ liệu, hoàn thành khi nhiệm vụ ban đầu được thực hiện hoặc xảy ra lỗi. Trong ví dụ này, suy luận đa lượt cho phép một luồng như:

1. Người dùng hỏi "liệt kê các nhân viên có kỹ năng liên quan đến AI"
2. LLM thấy rằng nó có quyền truy cập vào danh sách kỹ năng nhân viên và chỉ dẫn agent gọi công cụ đó
3. Agent gọi công cụ `get_skills` của nhân viên và trả về các kỹ năng cho LLM
4. LLM sau đó xác định kỹ năng nào liên quan đến AI và thấy rằng nó có thể lấy nhân viên với mỗi kỹ năng bằng cách sử dụng công cụ `get_employees_with_skill`
5. Agent lấy nhân viên cho mỗi kỹ năng và trả chúng lại cho LLM
6. LLM sau đó tập hợp danh sách đầy đủ các nhân viên có kỹ năng liên quan đến AI và trả về

Tương tác đa lượt này qua nhiều lần gọi LLM và nhiều lần gọi công cụ được đóng gói thành một lần gọi `agent(question)` duy nhất, cho thấy sức mạnh của Strands Agents thực hiện một vòng lặp agentic để đạt được nhiệm vụ được cung cấp. Ví dụ này cũng cho thấy cách employee agent có thể thêm các chỉ dẫn bổ sung trên các công cụ cơ bản, trong trường hợp này với một system prompt.

---

## Công khai một Agent dưới dạng MCP Server

Chúng ta có thể tương tác với agent này theo nhiều cách. Ví dụ, chúng ta có thể công khai nó như một dịch vụ REST. Trong trường hợp của chúng ta, chúng ta muốn công khai nó theo cách cho phép các agent khác tương tác với nó. Chúng ta có thể sử dụng MCP để tạo điều kiện giao tiếp giữa các agent đó bằng cách công khai agent này dưới dạng một MCP server. Sau đó, một agent khác (ví dụ: HR agent) sẽ có thể sử dụng employee agent như một công cụ.

Để công khai employee agent dưới dạng một MCP server, chúng ta chỉ cần bọc hàm `employee_agent` của chúng ta trong một `@mcp.tool`, chuyển đổi dữ liệu phản hồi thành một danh sách các chuỗi và khởi động server:

```python
mcp = FastMCP("employee-agent", stateless_http=True, host="0.0.0.0", port=8001)

@mcp.tool()
def inquire(question: str) -> list[str]:
    """answers questions related to our employees"""
    return [
        content["text"]
        for content in employee_agent(question).message["content"]
        if "text" in content
    ]

if __name__ == "__main__":
    mcp.run(transport="streamable-http")
```

Vì chúng ta đã bọc employee agent trong một MCP server, bây giờ chúng ta có thể sử dụng nó trong các agent khác. HR agent được công khai dưới dạng một REST API để chúng ta có thể gọi nó từ một ứng dụng web hoặc các dịch vụ khác.

```python
EMPLOYEE_AGENT_URL = os.environ.get("EMPLOYEE_AGENT_URL", "http://localhost:8001/mcp/")
hr_mcp_client = MCPClient(lambda: streamablehttp_client(EMPLOYEE_AGENT_URL))

bedrock_model = BedrockModel(
    model_id="amazon.nova-micro-v1:0",
    region_name="us-east-1",
    temperature=0.9,
)

app = FastAPI(title="HR Agent API")

class QuestionRequest(BaseModel):
    question: str

@app.get("/health")
def health_check():
    return {"status": "healthy"}

@app.post("/inquire")
async def ask_agent(request: QuestionRequest):
    async def generate():
        with hr_mcp_client:
            tools = hr_mcp_client.list_tools_sync()
            agent = Agent(model=bedrock_model, tools=tools, callback_handler=None)
            async for event in agent.stream_async(request.question):
                if "data" in event:
                    yield event["data"]

    return StreamingResponse(
        generate(),
        media_type="text/plain"
    )

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

---

## Triển khai MCP và Strands Agents trên AWS

Tất nhiên, chúng ta có thể chạy tất cả những điều này trên AWS bằng cách sử dụng nhiều tùy chọn khác nhau. Vì các MCP server sử dụng phương thức vận chuyển MCP Streamable HTTP mới, điều này có thể được chạy trên các runtime serverless như AWS Lambda hoặc AWS Fargate. Đối với ví dụ này, chúng tôi sẽ đóng gói employee info MCP server, employee agent và HR agent vào container, chạy chúng trên ECS và công khai HR agent thông qua một load balancer:

![Sơ đồ load balancer](https://d2908q01vomqb2.cloudfront.net/ca3512f4dfa95a03169c5a670a4c91a19b3077b4/2025/07/10/mermaid-diagram-2025-06-12-093700.png)

**MCP và Strands Agents trên Cơ sở Hạ tầng AWS**

Đối với ví dụ này, chúng tôi đã sử dụng AWS CloudFormation để định nghĩa cơ sở hạ tầng ([nguồn](https://github.com/aws-samples/Sample-Model-Context-Protocol-Demos/blob/main/modules/strands-mcp-inter-agent/infra.cfn)). Bây giờ với mọi thứ đang chạy trên AWS, chúng ta có thể thực hiện một yêu cầu đến HR agent:

```bash
curl -X POST --location "http://something.us-east-1.elb.amazonaws.com/inquire" \
-H "Content-Type: application/json" \
-d '{"question": "list employees that have skills related to AI programming"}'
```

Và chúng ta nhận được:

```
Here are the employees with skills related to AI programming:

1. W. Rodriguez - Machine Learning, REST API
2. M. Rodriguez - DevOps, Machine Learning, Python
3. R. Rodriguez - Machine Learning, JavaScript
4. J. Rodriguez - REST API, Kubernetes, Machine Learning, Node.js
5. W. Garcia - AWS, Kubernetes, GraphQL, Machine Learning
6. W. Davis - MongoDB, Angular, Kotlin, Machine Learning, REST API
7. J. Miller - React, Machine Learning, SQL, Kotlin
8. J. Rodriguez - SQL, Machine Learning, Docker, DevOps, Git

If you need more detailed information about any of these employees or require further assistance, please let me know!
```

Lấy [mã nguồn đầy đủ](https://github.com/aws-samples/Sample-Model-Context-Protocol-Demos/tree/main/modules/strands-mcp-inter-agent) cho ví dụ này.

---

## Đóng góp của AWS cho Khả năng Tương tác Tác nhân AI

Ví dụ này chỉ cho thấy phần bắt đầu của những gì chúng ta có thể làm với Strands Agents và MCP để giao tiếp giữa các agent. Chúng tôi đã làm việc với đặc tả MCP và các triển khai để giúp phát triển MCP nhằm hỗ trợ các khả năng bổ sung mà một số trường hợp sử dụng liên agent có thể cần.

Đặc tả MCP vừa có một [phiên bản mới 2025-06-18](https://modelcontextprotocol.io/specification/2025-06-18) bao gồm hai đóng góp từ AWS để hỗ trợ tốt hơn cho giao tiếp giữa các agent. Chúng tôi cũng đã đóng góp hỗ trợ cho các tính năng mới này trong nhiều triển khai MCP khác nhau.

### 1. Elicitation (Khai thác)

[Elicitation](https://modelcontextprotocol.io/specification/2025-06-18/client/elicitation): Khi các MCP server cần đầu vào bổ sung, chúng có thể báo hiệu điều đó cho agent (thông qua MCP client). Thay vì một lần gọi công cụ cung cấp phản hồi dữ liệu, nó có thể khai thác thông tin bổ sung. Ví dụ, nếu một Employee Info Agent sử dụng công cụ MCP để lấy dữ liệu nhân viên xác định rằng một yêu cầu công cụ như "lấy nhân viên có kỹ năng AI" có thể trả về nhiều kết quả hơn người dùng mong muốn, nó có thể khai thác rằng người dùng cung cấp tên nhóm để lọc. Cách tiếp cận này khác với một tham số công cụ vì các đặc điểm runtime có thể xác định rằng tên nhóm là cần thiết.

Trong một bài blog tương lai trong loạt bài này, chúng tôi sẽ đi sâu hơn vào cách sử dụng tính năng này. Chúng tôi đã đóng góp các triển khai của thay đổi đặc tả này cho các MCP SDK Java và Python, cả hai đều đã hợp nhất các thay đổi:
- [Thay đổi Java SDK](https://github.com/modelcontextprotocol/java-sdk/commit/8a5a591d39256ba3947003ec4477e1722363eb35)
- [Thay đổi Python SDK](https://github.com/modelcontextprotocol/python-sdk/pull/625)

### 2. Structured Output Schemas (Lược đồ Đầu ra có Cấu trúc)

[Structured Output Schemas](https://modelcontextprotocol.io/specification/2025-06-18/server/tools#structured-content): Các công cụ MCP thường trả về dữ liệu cho agent (thông qua MCP client) nhưng ngược lại với các lược đồ đầu vào (luôn được yêu cầu), chúng tôi đã đóng góp một cách để các công cụ tùy chọn chỉ định một lược đồ đầu ra. Điều này cho phép các agent thực hiện chuyển đổi an toàn hơn về kiểu dữ liệu của đầu ra công cụ thành dữ liệu có kiểu, cho phép các chuyển đổi an toàn và dễ dàng hơn của dữ liệu đó trong agent.

Chúng tôi cũng sẽ đề cập đến tính năng này nhiều hơn trong các bài blog tương lai trong loạt bài này. Các triển khai của tính năng này đã được đóng góp cho các MCP SDK Java, Python và TypeScript với triển khai TypeScript đã được hợp nhất:
- [Pull request Java SDK](https://github.com/modelcontextprotocol/java-sdk/pull/302)
- [Pull request Python SDK](https://github.com/modelcontextprotocol/python-sdk/pull/654)
- [Pull request TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk/pull/454)

---

## Kết luận

Thật thú vị khi thấy tiến bộ với MCP cho giao tiếp giữa các agent và trong các phần tương lai của loạt bài này, chúng tôi sẽ đi sâu hơn vào cách các cải tiến này có thể được sử dụng để cho phép các tương tác phong phú hơn giữa các agent.

Trong blog này, chúng ta đã thấy cách chúng ta có thể sử dụng Strands Agents và MCP để tạo một hệ thống các agent tất cả làm việc cùng nhau và chạy trên AWS. Strands Agents đã giúp dễ dàng xây dựng một agent kết nối với các công cụ MCP, và sau đó cũng công khai agent như một MCP server để các agent khác có thể giao tiếp với nó. Để bắt đầu tìm hiểu về Strands Agents, hãy xem [tài liệu](https://strandsagents.com/) và [repo GitHub](https://github.com/strands-agents/sdk-python). Hãy theo dõi để biết thêm các phần của loạt bài này về Khả năng Tương tác Agent!

---

## Về các Tác giả

### Nick Aldridge

![Nick Aldridge](https://d2908q01vomqb2.cloudfront.net/ca3512f4dfa95a03169c5a670a4c91a19b3077b4/2025/05/19/nick-profile-2.jpg)

Nick Aldridge là một Principal Engineer tại AWS. Trong 6 năm qua, Nick đã làm việc trên nhiều sáng kiến AI/ML bao gồm Amazon Lex và Amazon Bedrock. Gần đây nhất, anh ấy đã lãnh đạo nhóm ra mắt Amazon Bedrock Knowledge Bases. Ngày nay, anh ấy làm việc trên AI tổng quát và cơ sở hạ tầng AI với trọng tâm vào sự cộng tác giữa các agent và gọi hàm. Trước AWS, Nick đã có bằng MS tại Đại học Chicago.

### James Ward

James Ward là một Principal Developer Advocate tại AWS. James đi khắp thế giới giúp các nhà phát triển doanh nghiệp học cách xây dựng các hệ thống đáng tin cậy. Trọng tâm hiện tại của anh ấy là giúp các nhà phát triển xây dựng các hệ thống AI agent sử dụng Spring AI, Embabel, Strands Agents, Amazon Bedrock, MCP và A2A.

### Clare Liguori

![Clare Liguori](https://d2908q01vomqb2.cloudfront.net/ca3512f4dfa95a03169c5a670a4c91a19b3077b4/2022/05/23/JTqRDDF__200x200.jpg)

Clare Liguori là một Senior Principal Software Engineer cho AWS Agentic AI. Cô ấy tập trung vào việc tái tưởng tượng cách các ứng dụng được xây dựng và các nhà phát triển có thể hiệu quả như thế nào khi các công cụ của họ được hỗ trợ bởi AI tổng quát và AI agent, như một phần của Amazon Q Developer.

---

## Tài nguyên

- [Open Source tại AWS](https://aws.amazon.com/opensource)
- [Các Dự án trên GitHub](https://aws.github.io/)
- [Tài liệu Strands Agents](https://strandsagents.com/)
- [Kho lưu trữ GitHub Strands Agents](https://github.com/strands-agents/sdk-python)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [AWS Samples - Bản demo Agentic AI](https://github.com/aws-samples/sample-agentic-ai-demos/)

---

**Tags:** Trí tuệ Nhân tạo, Mã nguồn Mở, Model Context Protocol, Strands Agents, Amazon Bedrock, Hệ thống Đa Agent
