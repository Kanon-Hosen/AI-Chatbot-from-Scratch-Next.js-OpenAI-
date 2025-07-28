# AI Chatbot from Scratch | Next.js + OpenAI GPT-4o-mini

Welcome to the **AI Chatbot from Scratch** project!  
This repository provides a complete, step-by-step guide to building a smart, conversational AI chatbot backend using **Next.js** and **OpenAI's GPT-4o-mini** model — starting from zero, with no code cloning or complex setups.

---

## 🚀 Project Overview

This project demonstrates how to create a **context-aware AI chatbot backend** that:

- Uses **Next.js API routes** to handle chat requests  
- Integrates seamlessly with **OpenAI GPT** via the latest AI SDK  
- Maintains conversation context for a natural chat flow  
- Includes built-in error handling and API key management  
- Provides quick replies for enhanced user experience (customizable)  
- Offers a clean, modular codebase ideal for beginners and pros alike

---

## 📌 Why Build Your Own Chatbot?

- **Learn AI integration hands-on** without copying existing repos  
- Customize your chatbot’s personality and responses easily  
- Understand API route handling and serverless functions in Next.js  
- Gain experience working with OpenAI’s modern GPT models  
- Build a portfolio-ready project with real-world application  

---

## 🛠️ Features

- **Contextual conversation management:** Keeps last 5 messages for relevant replies  
- **Dynamic system prompt:** Customize chatbot personality & behavior  
- **Robust error handling:** Detects missing API keys and internal errors  
- **Quick reply suggestions:** Smart prompts based on user queries  
- **Simple frontend example:** React-based UI to get started fast  
- **Lightweight & efficient:** Uses `gpt-4o-mini` for balance of speed and quality  

---

## 📋 Prerequisites

- Basic JavaScript & React knowledge  
- Familiarity with Next.js (API routes and server components)  
- An active [OpenAI API key](https://platform.openai.com/account/api-keys)  

---

## ⚙️ Setup & Installation

1. **create your Next.js project:**

   ```bash
   npx create-next-app@latest my-ai-chatbot
   cd my-ai-chatbot
   ```

2. **Add your OpenAI API key:**  
   Create a `.env.local` file in the root folder and add:

   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   ```

3. **Install dependencies:**

   ```bash
   npm install ai @ai-sdk/openai
   ```

4. **Add API route:**  
   Create `app/api/chat/route.js` with the backend chatbot logic (see the code below).

5. **Start development server:**

   ```bash
   npm run dev
   ```

6. **Open** [http://localhost:3000](http://localhost:3000) to test your chatbot frontend.

---

## 💻 Backend Code (API Route)

```javascript
import { NextResponse } from "next/server";
import { generateText } from "ai";
import { openai } from "@ai-sdk/openai";

export async function POST(req) {
  try {
    if (!process.env.OPENAI_API_KEY) {
      return NextResponse.json(
        { reply: "API key missing. Please add it in .env.local" },
        { status: 500 }
      );
    }

    const { message, history, conversationId } = await req.json();

    const contextMessages =
      history?.slice(-5).map((msg) => ({
        role: msg.from === "user" ? "user" : "assistant",
        content: msg.text,
      })) || [];

    const { text } = await generateText({
      model: openai("gpt-4o-mini", {
        apiKey: process.env.OPENAI_API_KEY,
      }),
      system: `You are Kanon's intelligent portfolio assistant — a friendly, helpful, and professional AI chatbot designed to guide visitors through everything related to Kanon Hosen, a highly skilled Full Stack Web Developer from Bangladesh. You provide concise, conversational answers about Kanon's background, skills, tech stack, projects, achievements, services, experience, and how to get in touch. Always aim to be engaging, accurate, and easy to understand. If users ask vague questions like "Tell me about Kanon", introduce him briefly and offer to show skills, projects, or contact info. Your goal is to leave a positive, professional impression — like a smart virtual guide!`,
      messages: [...contextMessages, { role: "user", content: message }],
      maxTokens: 300,
      temperature: 0.8,
    });

    return NextResponse.json({
      reply: text,
      conversationId,
      timestamp: new Date().toISOString(),
    });
  } catch (error) {
    return NextResponse.json(
      { reply: "Oops, something went wrong. Please try again later.", error: error.message },
      { status: 500 }
    );
  }
}
```

---

## 🖥️ Frontend Example (React Component)

```jsx
"use client";
import { useState } from "react";

export default function Chat() {
  const [input, setInput] = useState("");
  const [messages, setMessages] = useState([]);

  async function sendMessage() {
    if (!input.trim()) return;

    const response = await fetch("/api/chat", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        message: input,
        history: messages,
        conversationId: "unique-conversation-id",
      }),
    });

    const data = await response.json();

    setMessages([
      ...messages,
      { from: "user", text: input },
      { from: "assistant", text: data.reply },
    ]);
    setInput("");
  }

  return (
    <>
      <div style={{ border: "1px solid #ccc", padding: "10px", height: "300px", overflowY: "auto", marginBottom: "12px" }}>
        {messages.map((msg, idx) => (
          <p key={idx} style={{ textAlign: msg.from === "user" ? "right" : "left" }}>
            <strong>{msg.from === "user" ? "You" : "Bot"}:</strong> {msg.text}
          </p>
        ))}
      </div>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        onKeyDown={(e) => e.key === "Enter" && sendMessage()}
        placeholder="Type a message..."
        style={{ width: "80%", padding: "10px" }}
      />
      <button onClick={sendMessage} style={{ padding: "10px 15px", marginLeft: "10px" }}>
        Send
      </button>
    </>
  );
}
```

---

## 🔍 How It Works

- **Frontend** sends user messages and conversation history to the backend API.  
- **Backend** collects recent chat messages, constructs context, and sends prompt to OpenAI GPT.  
- **OpenAI GPT** generates a relevant, context-aware response.  
- **Backend** returns the chatbot’s reply to the frontend.  
- **Frontend** updates the UI with the latest messages.

---

## 📈 SEO & Keywords

This project is ideal for:

- AI chatbot developers  
- Next.js learners  
- OpenAI GPT integration beginners  
- Full-stack JavaScript enthusiasts  


## 📬 Contact

**Kanon Hosen**  
- Email: [kanon5866@gmail.com](mailto:kanon5866@gmail.com)  
- LinkedIn: [linkedin.com/in/kanon-hosen](https://www.linkedin.com/in/kanon-hosen)  
- GitHub: [github.com/Kanon-Hosen](https://github.com/Kanon-Hosen)  

---

## 📝 License

This project is licensed under the [MIT License](LICENSE).

---

⭐ If this project helped you, please give it a ⭐ on GitHub!  
Happy coding! 🚀
