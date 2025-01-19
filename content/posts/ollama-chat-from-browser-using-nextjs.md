---
title: "Ollama Chat From Browser Using Nextjs"
date: 2024-11-21T00:31:49+07:00
draft: false
author: "prima adi"
author: "prima adi"
categories: ['technology', 'blog','ai']
tags: ['technology','ai', nextjs]
languageCode: 'en-us'
---

### Ollama Chat From Browser Using Nextjs

This writing will be so small.
it should be a step by step on creating nextjs app that will communicate with local ollama.
the model that i use is mistral its the fastest now running in my local.

but instead starting from scrath, lets asking help to [bolt.new](bolt.new)
to create interface that will communicate with our ollama server in local.

there is a reason why i learn this. i want to do .... lets wait for another post 😄

### bolt.new

this is [bolt.new](bolt.new) is actually worrying. i dont know this kind of software is good or not but its exist now.
little bit worried but still its awesome.

this is my prompt that i use to create the interface

```txt
create me a chat bot interface with dark theme using nextjs 14.
the chat will be using ollama.js that communicate with ollama server in my local.
```

why i ask for specific next js 14 because thats the one that i currently know so editing and adjusting the code will be fine.

![bolt.nw](/img/nextjs-ollama-1.png)

let bolt.new cook the bootstrap. and see from there.

- download from bolt.new
- run on local

and its failed and error 404 when hitting ollama/api/chat in.

`ollama serve`

```log
time=2024-11-21T00:52:25.492+07:00 level=INFO source=common.go:49 msg="Dynamic LLM libraries" runners=[metal]
time=2024-11-21T00:52:25.587+07:00 level=INFO source=types.go:123 msg="inference compute" id=0 library=metal variant="" compute="" driver=0.0 name="" total="5.3 GiB" available="5.3 GiB"
[GIN] 2024/11/21 - 00:52:32 | 404 |    5.942542ms |       127.0.0.1 | POST     "/api/chat"
```

but turns out i need to fix on the js since the model is not in my local

```ts
export default function Chat() {
  const [messages, setMessages] = useState<Message[]>([]);
  const [input, setInput] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!input.trim() || isLoading) return;

    const userMessage = { role: "user", content: input };
    setMessages((prev) => [...prev, userMessage]);
    setInput("");
    setIsLoading(true);

    try {
      const response = await fetch("http://localhost:11434/api/chat", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          model: "mistral", //generated code from bolt.new was llama2
          messages: [...messages, userMessage],
        }),
      });
```

and after debugging its still error. and like another normal person i am debug the issue and turnsout response from ollama not handled properly by bolt.new and need some adjustment. so ollama response per word is actually json. so we just neeed that on full text and show.

of course this is helped by ai. but like normal engineer i should understand what the code do.

- trim and split base on new line `\n`.
- map the lines and parse the line (the line already valid json) and trim.
- also join all the trimmed line to content.

this is response from ollama

```txt
{"model":"mistral","created_at":"2024-11-20T18:32:11.445645Z","message":{"role":"assistant","content":" No"},"done":false}
{"model":"mistral","created_at":"2024-11-20T18:32:11.476658Z","message":{"role":"assistant","content":","},"done":false}
{"model":"mistral","created_at":"2024-11-20T18:32:11.533155Z","message":{"role":"assistant","content":" I"},"done":false}
{"model":"mistral","created_at":"2024-11-20T18:32:11.590882Z","message":{"role":"assistant","content":" am"},"done":false}
```

```jsx
const lines = multilineResponse.trim().split("\n");
const content = lines
  .map(line => JSON.parse(line).message.content.trim()) // Parse each line and get the content
  .join(""); // Combine all content into a single string

const result = {
  role: "assistant",
  content: content
};

console.log(JSON.stringify(result, null, 2));
```

and

`No,I'mnotMistralAI.MistralAIisacutting-edgecompanybasedinParis,France,developinglargelanguagemodels.Iama`

haha trimming all the way. its trim all space in original line from ollama so i think we dont need to trim every content in per line
so we should adjust json parsing from ollama to

```jsx
const content = lines
  .map(line => JSON.parse(line).message.content) // Parse each line and get the content
  .join(""); // Combine all content into a single string
```

with this we should ok. lets see. i am writing this with debuging so its not **just work**
and walaaa its working now. i could chat with mistral from browser.

![bolt.nw](/img/nextjs-ollama-2.png)

next *term* that i want to explore is **RAG**.

## Realization

- bolt.new is really great helper, a lot of errors that still need to fix and adjust.
- bolt.new will perform better if we already know the context fully, like:
  - what payload from ollama explain it in detail with example
  - how i want to perform the output
- bolt.new sometimes generate unecessary files and component.
- mistral is the most 'OK' run in local.
- ollama is awesome tools. this /api/chat is awesome.
- transformer and do RAG will be next

see ya
