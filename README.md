# worker.js
export default {
  async fetch(request) {
    const BOT_TOKEN = "8704763332:AAG7B_POD4K2xEJMRWbwBR8nUL7mMeAtg24";

    if (request.method !== "POST") 
      return new Response("Bot Running ✅");

    const update = await request.json();
    if (!update.message?.text) return new Response("OK");

    const chatId = update.message.chat.id;
    const text = update.message.text;

    let reply = "Hello! 👋 Main tumhara Bot hoon!";

    await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
      method: "POST",
      headers: {"Content-Type": "application/json"},
      body: JSON.stringify({ chat_id: chatId, text: reply })
    });

    return new Response("OK");
  }
}
