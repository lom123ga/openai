# CAPABILITIES

## Text generation models

(Called generative pre-trained transformer or Large language models) 

Have been trained to understand natural language, code and images.

Input (referred to `prompt`)  ->  model -> Output (text)

To use one of these models via the OpenAI API, you send a request (inputs and API key), receive a resposne (models's output).

### Chat Completions

Take a list of messages as input and return a model-generated message output.

It is designed to make multi-turn

Ex: 

```python
from openai import OpenAI
client = OpenAI()

response = client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Who won the world series in 2020?"},
    {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
    {"role": "user", "content": "Where was it played?"}
  ]
)
```

- main input: messages parameter.

- messages : array of message object, each object has a role ("system" | "user" | "assistant") and content

- user message: provide request for the assistant to response to.

- assistant messages store previous assistant responses.

``Ex Response:``
```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "The 2020 World Series was played in Texas at Globe Life Field in Arlington.",
        "role": "assistant"
      },
      "logprobs": null
    }
  ],
  "created": 1677664795,
  "id": "chatcmpl-7QyqpwdfhqwajicIEznoc6Q47XAyW",
  "model": "gpt-3.5-turbo-0613",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 17,
    "prompt_tokens": 57,
    "total_tokens": 74
  }
}
```
Every response include a ``finish_reason`` :

- ``stop`` : return complete message or messagea terminated by one the stop sequences.

- ``length``:  incomplete model output due to ``max_tokens`` parameter or token limit

- ``function_call`` : decided to call a func

- ``content_filter`` : omitted content due to a flag from content filter

- ``null`` : API response still in process or incomplete

### response_format: 
```json
response_format={"type":"json_object"}
```
enable JSON mode

### Reproducible outputs

Set `seed` parameter for deterministic output

### Managing tokens

English: a token : one character or one word

Some language : a token can be > one word or < one character

Total number of tokens in an API call:

- pay per token

- input_token + output_token count in total

- Some model price per token is different for token in the input vs output.  [link](https://openai.com/pricing)

- count token in text string: [tiktoken](https://github.com/openai/tiktoken)

- Number of token = content + role + field + a few extra for behind-the -scenes formatting.

- If a conversation has too many token (> maximun limit) , you will have to truncate, omit, ... to fit it.

### Completion API (Legacy)

input: a freeform text string call a `prompt`.

[Query]
```python
from openai import OpenAI
client = OpenAI()

response = client.completions.create(
  model="gpt-3.5-turbo-instruct",
  prompt="Write a tagline for an ice cream shop."
)
```

[Response]
```python
{
  "choices": [
    {
      "finish_reason": "length",
      "index": 0,
      "logprobs": null,
      "text": "\n\n\"Let Your Sweet Tooth Run Wild at Our Creamy Ice Cream Shack"
    }
  ],
  "created": 1683130927,
  "id": "cmpl-7C9Wxi9Du4j1lQjdjhxBlO22M61LD",
  "model": "gpt-3.5-turbo-instruct",
  "object": "text_completion",
  "usage": {
    "completion_tokens": 16,
    "prompt_tokens": 10,
    "total_tokens": 26
  }
}
```

### Which model should I use ?

recommend that you use either `gpt-4` or `gpt-3.5-turbo`

Some note:

- Context window :

+ gpt4 : Maximun size 8.192 tokens

+ pgt-3.5-turbo: 4.096 tokens

## Function Calling

# Pricing

| Model | Inout | Output|
|-------|-------|-------|
|gpt-4  |$0.03 / 1K tokens|$0.06 / 1K token|
|gpt-4-0125-preview | $0.01/1K tokens | $0.03/1K tokens|
|gpt-3.5-turbo-1106|$0.00010/1K tokens|0.0020/1K tokens|

`Assistants API`
|Tool | Inout|
|-----|------|
|code interpreter|$0.03/session|
|retrieval|$0.20/GB/assistant/day|

`Fine-turning models`
|Model|Trainig|input|output|
|-----|-------|------|------|
|gpt-3.5-turbo|$0.0080/1K tokens|0.0030/1K tokens|$0.0060/1K tokens|
|davinci-002|0.006/1K tokens|0.0120/1K tokens|0.0120/1K tokens|
|babbage-002|$0.0004/1K tokens| $0.0016/1K tokens|$0.0016/1K tokens|

`Embedding`
