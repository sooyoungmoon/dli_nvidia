
## Function calling
함수 호출(Function Calling)은 특정 작업을 수행하기 위해 미리 정의된 함수를 사용하는 것

### System, user, assistance
System: 선택 사항,  어시스턴트의 동작을 설정하는 데 사용
User: 사용자가 직접 입력하는 메시지
Assistance: System이 설정한 태도와 설정을 반영하여 사용자의 요청사항에 답변

## How to use function calling

### Step 1: 함수를 정의한다
get_fruit_price: 과일 이름을 입력하면 과일 가격을 반환하는 함수

```python
def get_fruit_price(fruit_name):
fruit_prices = {
'사과': '1000',
'바나나': '500',
'오렌지': '750',
'배': '800'
}
if fruit_name in fruit_prices:
	return fruit_prices[fruit_name]
```
### Step 2: 함수 설명을 작성한다
함수에 대한 설명을 형식에 맞춰 작성한다
```python
use_functions = [
   {
       "type": "function",
       "function":{
           "name": "get_fruit_price", # 함수 이름
           "description": "Returns the current price of the specified fruit.", # 함수에 대한 설명
           "parameters": {
               "type": "object",
               "properties": {
		    # 함수에서 인자 설명
                   "fruit_name": {
                       "type": "string",
                       "description": "The name of the fruit to retrieve the price for (e.g., '사과', '바나나')."
                   }
               },
               "required": ["fruit_name"] 
           }
       }
   }
]

```

### Step 3: GPT에게 질문해서 어떤 함수를 호출할 지 알아낸다
```python
messages = [
{"role": "system", "content": "You are a helpful assistant. Use the supplied tools to retrieve fruit prices for the user."},
{"role": "user", "content": "Hi, can you tell me the price of 3 apples?"}
]

client = OpenAI()

response = client.chat.completions.create(
model="gpt-4o-mini",
messages=messages,
tools=use_functions
)

response_message = response.choices[0].message
tool_calls = response_message.tool_calls

```

### Step 4: 함수를 실행시킨다
```python
proc_messages = []

if tool_calls:
available_functions = {
"get_fruit_price": get_fruit_price
}
for tool_call in tool_calls:
messages.append(response_message) 

function_name = tool_call.function.name
function_to_call = available_functions[function_name]
function_args = json.loads(tool_call.function.arguments)

function_response = function_to_call(**function_args)  # 함수 실행

proc_messages.append(
{
"tool_call_id": tool_call.id,
"role": "tool",
"name": function_name,
"content": function_response,
}
)
messages += proc_messages

```

### Migration issue 
[다음 링크 참조] (https://github.com/openai/openai-python/discussions/742)
