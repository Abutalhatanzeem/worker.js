# worker.js
export default {
  async fetch(request) {
    
    // ===== APNI KEYS YAHAN DAALO =====
    const BOT_TOKEN = "8704763332:AAG7B_POD4K2xEJMRWbwBR8nUL7mMeAtg24";
    const GROQ_API_KEY = "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";
    const MODEL = "llama3-70b-8192"; // Best free model
    // ==================================

    if (request.method !== "POST") return new Response("Bot Running ✅");

    const update = await request.json();
    if (!update.message?.text) return new Response("OK");

    const chatId = update.message.chat.id;
    const userMessage = update.message.text;

    // Groq API Call
    const groqResponse = await fetch("https://api.groq.com/openai/v1/chat/completions", {
      method: "POST",
      headers: {
        "Authorization": `Bearer ${GROQ_API_KEY}`,
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        model: MODEL,
        messages: [
          {
            role: "system",
            content: "Tum ek helpful assistant ho. Short aur clear jawab do."
          },
          {
            role: "user",
            content: userMessage
          }
        ],
        max_tokens: 500
      })
    });

    const groqData = await groqResponse.json();
    const reply = groqData.choices?.[0]?.message?.content || "Kuch gadbad hui ⚠️";

    // Telegram ko reply bhejo
    await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        chat_id: chatId,
        text: reply
      })
    });

    return new Response("OK");
  }
}
