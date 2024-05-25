langchain: LLM(Large language model)을 활용한 어플리케이션을 만들기 위한 프레임워크, memory를 위한 모듈 존재,

모든 필요요소들의 호환을 위한 계층, clade를 사용할려면 한줄만 바꾸면 된다.

langsmith라는 디버거, 

github 툴을 agent에 연결하거나 wikipedia tool을 연결할 수 도 있음

프론트: streamlit

html, css, javascript 필요없이 python코드만 있으면 된다.

pinecone: Vector를 위한 database 같은 거

폴더 생성 - git init . - .gitignore, [README.md](http://README.md) 생성

virtual environment 생성

python -m venv ./env

.gitignore에 env/ 추가

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/c371114d-0c61-4247-bc94-cf672e165984/c70aece6-4da6-4dc2-8aae-2925e52bf013/Untitled.png)

운영체제에 따라 입력

```jsx
env\Scripts\Activate.ps1 : 이 시스템에서 스크립트를 실행할 수 없으므로 C:\FULLSTACK_GPT\env\Scripts\Activate.ps1 파일을 로드할 수 없습니다. 자세한
내용은 about_Execution_Policies(https://go.microsoft.com/fwlink/?LinkID=135170)를 참조하십시오.
위치 줄:1 문자:1- env\Scripts\Activate.ps1
- `+ CategoryInfo : 보안 오류: (:) [], PSSecurityException + FullyQualifiedErrorId : UnauthorizedAccess`
```

- 

이런 오류가 뜬다면 powershell을 관리자 권한으로 실행해서

 

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process

안되면 

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

안되면

Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

바로 위 방법은 모든 스크립트와 구성 파일을 실행할 수 있게 해주므로 보안에 취약해질 수 있어서 주의 요함 

requirements.txt 파일 생성 

https://gist.github.com/serranoarevalo/72d77c36dde1cc3ffec34105eb666140
내용 붙여넣기 

pip install -r requirements.txt

Microsoft Visual C++ 14.0 이상이 필요하다는 오류가 발생

https://visualstudio.microsoft.com/ko/visual-cpp-build-tools/

C++빌드도구 설치

ModuleNotFoundError: No module named 'distutils’

설치 과정에서 발생한 **`distutils`** 모듈 관련 문제는 Python 3.12에서 **`distutils`**가 더 이상 사용되지 않기 때문입니다. Python 3.10 이상에서는 **`distutils`** 모듈이 제거되어 이와 관련된 오류가 발생.

https://www.python.org/downloads/release/python-3116/

3.11.6설치

bash로 가상환경 설치하니 해결

[main.py](http://main.py) 생성

```python
import tiktoken

print(tiktoken)
```

이후 python [main.py](http://main.py)

<module 'tiktoken' from 'C:\\FULLSTACK\\.venv\\Lib\\site-packages\\tiktoken\\**init**.py'>

이렇게 뜨면 정상

디렉토리에 .env생성하여(.gitignore에 필수 추가)

OPENAI_API_KEY=”” key 추가

로컬(PC)에서 프로젝트를 이미 생성한 상태이고, Github에 올리기 위해

1. 버전 관리를 원하는 로컬 저장소(폴더)에서 **git init** 입력
2. **Github에서 원격 저장소 생성**
3. **git add .**
4. **git commit -m 'first commit'**
5. **git branch -M main**
6. **git remote add origin {원격 저장소 주소}**
7. **git push -u origin main**

위 명령어들을 Git Bash에서 입력

langchain 맛보기

ipynb만들어서

```python
from langchain.llms.openai import OpenAI
from langchain.chat_models import ChatOpenAI

llm = OpenAI()

chat = ChatOpenAI

a = llm.predict("How many palnets are there?")
b = chat.predict("How many palnets are there?")

a, b
```

InvalidRequestError: The model `text-davinci-003` has been deprecated오류

llm = OpenAI(model_name="gpt-3.5-turbo-1106")

chat = ChatOpenAI(model_name="gpt-3.5-turbo-1106")로 수정

('There are 8 planets in our solar system: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, and Neptune.',
'As of now, there are eight recognized planets in our solar system: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, and Neptune. However, there is ongoing debate and research regarding the classification of other celestial bodies as planets, so this number may change in the future.')

이게 뜨면 정상

temperature=0.1 gpt의 값이 낮을수록 창의성 낮아짐

- code1
    
    ```python
    from langchain.schema import HumanMessage, AIMessage, SystemMessage
    
    messages = [
        SystemMessage(
            content="You are a geography expert. And you only reply in Korean.",
        ),
        AIMessage(content="안녕하세요 제 이름은 사나래입니다.!"),
        HumanMessage(content="멕시코와 태국 사이의 거리는 어떻게 되나요? 그리고 당신의 이름은 무엇인가요?",
        ),
    ]
    
    chat.predict_messages(messages)
    ```
    
- code2
    
    ```python
    from langchain.schema import HumanMessage, AIMessage, SystemMessage
    
    messages = [
        SystemMessage(
            content="You are a geography expert. And you only reply in {language}.",
        ),
        AIMessage(content="안녕하세요 제 이름은 {name}입니다.!"),
        HumanMessage(content="{country_a}와 {country_b} 사이의 거리는 어떻게 되나요? 그리고 당신의 이름은 무엇인가요?",
        ),
    ]
    
    chat.predict_messages(messages)
    ```
    
- code3
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts import PromptTemplate, ChatPromptTemplate
    
    template = PromptTemplate.from_template("{country_a}와 {country_b} 사이의 거리는 어떻게 되나요?")
    
    prompt = template.format(country_a="Korea", country_b="Mexico")
    
    chat = ChatOpenAI(
        temperature=0.1
    )
    
    chat.predict(prompt)
    ```
    
    ```python
    
    template = ChatPromptTemplate.from_messages([
        ("system", "You are a geography expert. And you only reply in {language}."),
        ("ai", "안녕하세요 제 이름은 {name}입니다.!"),
        ("human", "{country_a}와 {country_b} 사이의 거리는 어떻게 되나요? 그리고 당신의 이름은 무엇인가요?")
        
    ])
    
    prompt=template.format_messages(
        language="Greek",
        name="anu",
        country_a="Mexico",
        country_b="Thailand",
    )
    
    chat.predict_messages(prompt)
    ```
    
- code4
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts import ChatPromptTemplate
    
    chat = ChatOpenAI(temperature=0.1)
    ```
    
    ```python
    from langchain.schema import BaseOutputParser
    
    class CommaOutputParser(BaseOutputParser):
    
        def parse(self, text):
            items = text.strip().split(",")
            return list(map(str.strip,items))
    ```
    
    ```python
    template = ChatPromptTemplate.from_messages([
        ("system", "You are a list generating machine. Everything you are asked will be answered with a comma separated list of max {max_items} in lowercase. Do NOT reply with anything else."),
        ("human", "{question}"),
    ]
    )
    
    prompt = template.format_messages(
        max_items=10,
        question="What are the colors?"
    )
    
    result = chat.predict_messages(prompt)
    
    p = CommaOutputParser()
    p.parse(result.content)
    ```
    
- code5
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts import ChatPromptTemplate
    
    chat = ChatOpenAI(temperature=0.1)
    ```
    
    ```python
    from langchain.schema import BaseOutputParser
    
    class CommaOutputParser(BaseOutputParser):
    
        def parse(self, text):
            items = text.strip().split(",")
            return list(map(str.strip,items))
    ```
    
    ```python
    template = ChatPromptTemplate.from_messages([
        ("system", "You are a list generating machine. Everything you are asked will be answered with a comma separated list of max {max_items} in lowercase. Do NOT reply with anything else."),
        ("human", "{question}"),
    	]
    )
    ```
    
    ```python
    chain = template | chat | CommaOutputParser()
    
    chain.invoke({
        "max_items":5,
        "question":"What are the pokemons?"
    })
    ```
    
    chain예시
    
    ```python
    chain_one = template | chat | CommaOutputParser()
    
    chain_two = template_2 | chat | outputparser_1
    
    all = chain_one | chain_two | outputparser_2
    
    ```
    
- code6
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts import ChatPromptTemplate
    from langchain.callbacks import StreamingStdOutCallbackHandler
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    chef_prompt = ChatPromptTemplate.from_messages([
        ("system", "당신은 월드 클래스 국제셰프입니다. 당신은 어떤 종류의 요리든 쉽게 구할 수 있는 재료로 따라하기 쉬운 레시피를 만들어줍니다."),
        ("human", "저는 {cuisine} 요리를 요리하고 싶어요"),
    ])
    
    chef_chain = chef_prompt | chat
    
    ```
    
    ```python
    veg_chef_prompt = ChatPromptTemplate.from_messages([
        ("system", "당신은 채식주의자를 위한 셰프입니다. 전통적인 채식주의자용 레시피에 특화되어 있습니다. 당신은 대체 재료를 찾고, 준비하는 방법에 대해 설명합니다. 기존 레시피를 너무 많이 변경해서는 안됩니다. 만약 다른 대체품이 없다면 그냥 레시피를 모른다고 말하세요."),
        ("human", "{recipe}")
    ])
    
    veg_chain = veg_chef_prompt | chat
    
    final_chain = {"recipe":chef_chain} | veg_chain
    
    final_chain.invoke({
        "cuisine":"korea"
    })
    ```
    
- code7
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts.few_shot import FewShotPromptTemplate
    from langchain.callbacks import StreamingStdOutCallbackHandler
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    examples = [
    {
    "question": "What do you know about France?",
    "answer": """
    Here is what I know:
    Capital: Paris
    Language: French
    Food: Wine and Cheese
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Italy?",
    "answer": """
    I know this:
    Capital: Rome
    Language: Italian
    Food: Pizza and Pasta
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Greece?",
    "answer": """
    I know this:
    Capital: Athens
    Language: Greek
    Food: Souvlaki and Feta Cheese
    Currency: Euro
    """,
    },
    ]
    
    example_template = """
        Human: {question}
        AI: {answer}
    """
    
    example_prompt = PromptTemplate.from_template("Human: {question}\nAI:{answer}")
    
    prompt = FewShotPromptTemplate(
        example_prompt=example_prompt,
        examples=examples,
        suffix="Humanm: What do you know about {country}?",
        input_variables=["country"]
    )
    
    chain=prompt|chat
    
    chain.invoke({
        "country": "Korea"
    })
    ```
    
- code8
    
    FewShotPromptTemplate 예제를 입력해서 원하는 형식을 얻기 위해 사용
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts.few_shot import FewShotPromptTemplate, FewShotChatMessagePromptTemplate
    from langchain.callbacks import StreamingStdOutCallbackHandler
    from langchain.prompts import ChatPromptTemplate
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    examples = [
    {
    "country": "France",
    "answer": """
    Here is what I know:
    Capital: Paris
    Language: French
    Food: Wine and Cheese
    Currency: Euro
    """,
    },
    {
    "country": "Italy",
    "answer": """
    I know this:
    Capital: Rome
    Language: Italian
    Food: Pizza and Pasta
    Currency: Euro
    """,
    },
    {
    "country": "Greece",
    "answer": """
    I know this:
    Capital: Athens
    Language: Greek
    Food: Souvlaki and Feta Cheese
    Currency: Euro
    """,
    },
    ]
    
    example_prompt = ChatPromptTemplate.from_messages([
        ("human", "What do you know about {country}?"),
        ("ai", "{answer}")
    ])
    
    example_prompt = FewShotChatMessagePromptTemplate(
        example_prompt=example_prompt,
        examples=examples,
    )
    
    final_prompt = ChatPromptTemplate.from_messages([
        ("system", "You are a geography expert, you give short answers."),
        example_prompt,
        ("human", "What do you know about {country}?")
    
    ])
    
    chain = final_prompt | chat
    
    chain.invoke({
        "country": "South Korea"
    })
    ```
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts.few_shot import FewShotPromptTemplate
    from langchain.callbacks import StreamingStdOutCallbackHandler
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    examples = [
    {
    "question": "What do you know about France?",
    "answer": """
    Here is what I know:
    Capital: Paris
    Language: French
    Food: Wine and Cheese
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Italy?",
    "answer": """
    I know this:
    Capital: Rome
    Language: Italian
    Food: Pizza and Pasta
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Greece?",
    "answer": """
    I know this:
    Capital: Athens
    Language: Greek
    Food: Souvlaki and Feta Cheese
    Currency: Euro
    """,
    },
    ]
    
    example_template = """
        Human: {question}
        AI: {answer}
    """
    
    example_prompt = PromptTemplate.from_template("Human: {question}\nAI:{answer}")
    
    prompt = FewShotPromptTemplate(
        example_prompt=example_prompt,
        examples=examples,
        suffix="Humanm: What do you know about {country}?",
        input_variables=["country"]
    )
    
    chain=prompt|chat
    
    chain.invoke({
        "country": "Korea"
    })
    ```
    
- code9
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts.few_shot import FewShotPromptTemplate
    from langchain.callbacks import StreamingStdOutCallbackHandler
    from langchain.prompts.prompt import PromptTemplate
    from langchain.prompts.example_selector import LengthBasedExampleSelector
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    examples = [
    {
    "question": "What do you know about France?",
    "answer": """
    Here is what I know:
    Capital: Paris
    Language: French
    Food: Wine and Cheese
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Italy?",
    "answer": """
    I know this:
    Capital: Rome
    Language: Italian
    Food: Pizza and Pasta
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Greece?",
    "answer": """
    I know this:
    Capital: Athens
    Language: Greek
    Food: Souvlaki and Feta Cheese
    Currency: Euro
    """,
    },
    ]
    
    example_prompt = PromptTemplate.from_template(
        ("Human: {question}\nAI:{answer}")
    )
    
    example_selector = LengthBasedExampleSelector(
        examples=examples,
        example_prompt=example_prompt,
        max_length=80,
    )
    
    prompt = FewShotPromptTemplate(
        example_prompt=example_prompt,
        example_selector=example_selector,
        suffix="Human: What do you know about {country}?",
        input_variables=["country"]
    )
    
    prompt.format(country="Brazil")
    ```
    
- code10
    
    ```python
    from typing import Any, Dict
    from langchain.chat_models import ChatOpenAI
    from langchain.prompts import example_selector
    from langchain.prompts.few_shot import FewShotPromptTemplate
    from langchain.callbacks import StreamingStdOutCallbackHandler
    from langchain.prompts.prompt import PromptTemplate
    from langchain.prompts.example_selector.base import BaseExampleSelector
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    examples = [
    {
    "question": "What do you know about France?",
    "answer": """
    Here is what I know:
    Capital: Paris
    Language: French
    Food: Wine and Cheese
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Italy?",
    "answer": """
    I know this:
    Capital: Rome
    Language: Italian
    Food: Pizza and Pasta
    Currency: Euro
    """,
    },
    {
    "question": "What do you know about Greece?",
    "answer": """
    I know this:
    Capital: Athens
    Language: Greek
    Food: Souvlaki and Feta Cheese
    Currency: Euro
    """,
    },
    ]
    
    class RandomExampleSelector(BaseExampleSelector):
    
        def __init__(self, examples):
            self.examples=examples
    
        def add_example(self, example):
            self.examples.append(example)
    
        def select_examples(self, input_variables):
            from random import choice
            return [choice(self.examples)]
    
    example_prompt = PromptTemplate.from_template(
        ("Human: {question}\nAI:{answer}")
    )
    
    example_selector = RandomExampleSelector(
        examples=examples,
       
    )
    
    prompt = FewShotPromptTemplate(
        example_prompt=example_prompt,
        example_selector=example_selector,
        suffix="Human: What do you know about {country}?",
        input_variables=["country"]
    )
    
    prompt.format(country="Brazil")
    ```
    
- code11
    
    yaml이랑 json파일로 prompt 별도로 관리
    
    ```python
    from langchain.chat_models import ChatOpenAI
    from langchain.callbacks import StreamingStdOutCallbackHandler
    from langchain.prompts import load_prompt
    
    prompt = load_prompt("./prompt.yaml") #.json
    
    chat = ChatOpenAI(temperature=0.1, streaming=True, callbacks=[StreamingStdOutCallbackHandler(),],)
    
    prompt.format(country="Korea")
    ```
    
    ```json
    {
        "_type":"prompt",
        "template": "What is the capital of {country}",
        "input_variables": [
            "country"
        ]
    }
    ```
    
    ```yaml
    _type: "prompt"
    template: "What is the capital of {country}"
    input_variables: ["country"]
    ```
    

chefgpt

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/c371114d-0c61-4247-bc94-cf672e165984/290ed9a4-d8ef-4bee-800b-7558571b5aca/Untitled.png)

winget설치후 콘솔에 명령어 입력

설치후 콘솔에 cloudflared help 입력 시 뭐가 주르륵 뜨면 성공 

pip install fastapi

main.py만들고

```python
from fastapi import FastAPI

app = FastAPI(
    title="SANARE",
    description="A meal planning assitant for elderly people"
)

@app.get("/plan")
def get_plan():
    return{"plan": "과일은 당뇨환자에게 좋지않다."}
```

가상환경 콘솔에

uvicorn main:app --reload 

127.0.0.1:8000처럼 나와있는 주소로 들어가서 /plan 붙여주면 짠 

http://127.0.0.1:8000/docs

[http://127.0.0.1:8000/](http://127.0.0.1:8000/docs)openapi.json

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI(
    title="SANARE",
    description="A meal planning assitant for elderly people"
)

class Plan(BaseModel):
    plan: str = Field(description="사나래가 말한 레시피")
    health: str = Field(description="사나래가 말한 레시피의 건강에 대한 영향")

@app.get("/plan", summary="이 endpoint는 사나래의 레시피를 무작위로 return합니다.", description = "GET request를 받으면 이 endpoint는 사나래의 사용자 맞춤 레시피를 return합니다.", response_description="Plan object로 응답할 것이고 object는 사나래가 말한 레시피와 건강에 대한 영향을 포함합니다.", response_model=Plan)
def get_plan():
    return{"plan": "사나래의 사용자 맞춤 레시피.", "health": "건강에 대한 영향"}
```

새 콘솔 열어서 가상환경에

cloudflared tunnel --url [http://127.0.0.1:8000](http://127.0.0.1:8000/)

url에 /openapi.json 붙여서 작동하는지 확인

```python
app = FastAPI(
    title="SANARE",
    description="A meal planning assitant for elderly people", servers=[
        {"url":"https://bring-html-adoption-reproduced.trycloudflare.com"
        }
     ]
 )
```

gpt설정에 action들어가서 import url눌러서 붙여넣고 import

privacy policy에도 주소 붙여넣기 

openapi_extra={"x-openapi-isConsequential": True}

True면 허용하기, 거부하기만 있음

openapi_extra={"x-openapi-isConsequential": False}

False면 항상 허용이 추가

Authentication

API Key: GPT가 API에 요청하는 때를 알기 위해 사용

server에 openai만 접근 가능한 url이 있다면 API Key사용

OAuth: GPT에게 이야기하고 있는 사용자가 누구인지 알고 싶을 때 사용 

```python
def get_plan(request: Request):
    print(request.headers)
    return{"plan": "사나래의 사용자 맞춤 레시피.", "health": "건강에 대한 영향"}
```

gpt를 사용하면

'authorization': 'Bearer 2379’

이런식으로 콘솔에 뜸

OAuth

Authorization URL:

https://bring-html-adoption-reproduced.trycloudflare.com/authorize

Token URL:

[https://bring-html-adoption-reproduced.trycloudflare.com/](https://bring-html-adoption-reproduced.trycloudflare.com/authorize)token

Scope:

user:read, user:delete

```python
@app.get("/authorize")
def handle_authorize(response_type: str, client_id: str, redirect_uri: str, scope: str, state: str):
    print(
        response_type,
        client_id,
        redirect_uri,
        scope,
        state,
    )
    return{
        "ok": True,
    }
```

```python
from fastapi import FastAPI, Request, Body, Form
from pydantic import BaseModel, Field
from fastapi.responses import HTMLResponse
from typing import Any

app = FastAPI(
    title="SANARE",
    description="A meal planning assitant for elderly people",
    servers=[
        {"url":"https://bring-html-adoption-reproduced.trycloudflare.com"
        }
    ]
)

class Plan(BaseModel):
    plan: str = Field(description="사나래가 건강상태를 기반으로 먹어도 되는 음식 추천")
    health: str = Field(description="건강상태")

@app.get("/plan", summary="이 endpoint는 사나래의 음식 추천을 무작위로 return합니다.", description = "GET request를 받으면 이 endpoint는 사나래의 사용자 맞춤 음식을 return합니다.", response_description="Plan object로 응답할 것이고 object는 사나래가 말한 음식과 건강에 대한 영향을 포함합니다.", response_model=Plan, openapi_extra={"x-openapi-isConsequential": False})
def get_plan(request: Request):
    print(request.headers)
    return{"plan": "사나래의 사용자 맞춤 음식 추천.", "health": "당뇨"}

user_token_db = {
    "ABCDEF": "anu"
}

@app.get("/authorize", response_class=HTMLResponse,)
def handle_authorize(client_id: str, redirect_uri: str, state: str):
    return f"""
    <html>
        <head>
            <title>SANARE Log In</title>
        </head>
        <body>
            <h1>Log Into SANARE</h1>
            <a href="{redirect_uri}?code=ABCDEF&state={state}">Authorize SANARE GPT</a>
        </body>
    </html>
    """

@app.post("/token")
def handle_token(code = Form(...)):
        print(code)
        return {
             "access_token":user_token_db[code]
             
        }
```

```python
from fastapi import FastAPI, Request, Body, Form
from pydantic import BaseModel, Field
from fastapi.responses import HTMLResponse
from typing import Any

app = FastAPI(
    title="SANARE",
    description="A meal planning assitant for elderly people",
    servers=[
        {"url":"https://bring-html-adoption-reproduced.trycloudflare.com"
        }
    ]
)

class Plan(BaseModel):
    plan: str = Field(description="사나래가 건강상태를 기반으로 먹어도 되는 음식 추천")
    health: str = Field(description="건강상태")

@app.get("/plan", summary="이 endpoint는 사나래의 음식 추천을 무작위로 return합니다.", description = "GET request를 받으면 이 endpoint는 사나래의 사용자 맞춤 음식을 return합니다.", response_description="Plan object로 응답할 것이고 object는 사나래가 말한 음식과 건강에 대한 영향을 포함합니다.", response_model=Plan, openapi_extra={"x-openapi-isConsequential": False})
def get_plan(request: Request):
    print(request.headers)
    return{"plan": "사나래의 사용자 맞춤 음식 추천.", "health": "당뇨"}

user_token_db = {
    "ABCDEF": "anu"
}

@app.get("/authorize", response_class=HTMLResponse, include_in_schema=False,)
def handle_authorize(client_id: str, redirect_uri: str, state: str):
    return f"""
    <html>
        <head>
            <title>SANARE Log In</title>
        </head>
        <body>
            <h1>Log Into SANARE</h1>
            <a href="{redirect_uri}?code=ABCDEF&state={state}">Authorize SANARE GPT</a>
        </body>
    </html>
    """

@app.post("/token", include_in_schema=False,)
def handle_token(code = Form(...)):
        print(code)
        return {
             "access_token":user_token_db[code]
        }
```

%pip install --upgrade --quiet  langchain-pinecone langchain-openai langchain

```python

from langchain.document_loaders import CSVLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from pinecone import Pinecone
from langchain_pinecone import PineconeVectorStore

pc = Pinecone(api_key="PINECONE_API_KEY")

splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder()

loader = CSVLoader("./recipes.csv", encoding="utf-8")

docs = loader.load_and_split(text_splitter=splitter)

embeddings = OpenAIEmbeddings()

vector_store = PineconeVectorStore.from_documents(docs, embeddings, index_name="recipes")
```
