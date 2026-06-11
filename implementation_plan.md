# Plan to Implement the AI Study Companion

This plan outlines the design and integration of the AI **Study Companion** tab inside "The Quiet Place". It connects the client frontend directly to the Google Gemini API, providing context-aware answers with offline fallbacks.

## User Review Required

> [!IMPORTANT]
> **API Key Safety:** To make the AI conversational, the user can configure their personal **Google Gemini API Key**. This key is saved solely on the client side inside the browser's `localStorage` (it is never sent to any middleman server).
> 
> **Offline/Mock Mode:** If the user does not have a key, the Companion operates in a high-quality "Offline Mode" which responds with pre-authored scholarly insights for the current chapter whenever they click suggestion chips.

## Proposed Changes

### [The Quiet Place Frontend](file:///e:/Shasha/index.html)

#### [MODIFY] [index.html](file:///e:/Shasha/index.html)

1. **HTML Layout Updates:**
   - Replace the static placeholder inside `<div id="chatbot">` with a chat structure:
     - **Header:** Status indicator ("Offline Mode" vs "Connected"), active book/chapter context, and settings gear.
     - **Settings Pane:** Secure input field for Gemini API key.
     - **Message Pane:** Chat scroll-box displaying message dialogues.
     - **Suggestion Pane:** Dynamic chips prompting the AI (Historical Context, Greek/Hebrew, Cross References, Life Application).
     - **Input Box:** Text field and send button.
2. **CSS Layout & Styling:**
   - Add chat CSS: `.chat-layout`, `.chat-messages`, `.chat-bubble`, `.typing-indicator`, `.suggestion-chip`, and `.chat-pin-btn`.
   - Style user bubbles with warm earth tones, and Companion bubbles with soft grey-green sage tones.
3. **JavaScript API Integration:**
   - Implement client keys: `toggleApiKeyPanel()`, `saveApiKey()`, and `clearApiKey()`.
   - Implement DOM scripture scraper `getActiveScriptureTextForAI()` to send loaded text context directly to Gemini.
   - Implement `sendChatMessage()` to handle user sends, input locking, and scroll anchors.
   - Implement REST payload request utilizing `fetch()` to the Gemini `v1beta` models endpoint (`gemini-1.5-flash:generateContent`).
   - Implement conversational history logic (`chatMessagesHistory`) to enable multi-turn dialogues.
   - Implement mock response matching for offline state.
   - Implement `copyChatToJournal(text)` to let users append AI insights straight into their current Journal draft in blockquote format.

---

## Verification Plan

### Manual Verification
1. **Initial Open:** Go to the **Companion** tab. Verify the status reads "● Offline Mode" and the active scripture matches the selector (e.g. "Genesis 1").
2. **Offline Suggestions Test:** Click the "🏛️ Historical Context" chip. Verify:
   - A user bubble is created with the request.
   - A temporary typing indicator (`. . .`) bounces.
   - A mock scholarly response appears explaining Genesis 1 context, along with a "📋 Send to Journal" action.
3. **Pin Chat to Journal:** Click "📋 Send to Journal" on the response. Go to the **Journal** tab and verify the insight is inserted.
4. **Set API Key:** Click the gear icon, paste a valid Gemini key, and click Save. Verify the status changes to "● Connected".
5. **Real-time Conversation Test:** Ask a custom question (e.g., *"What is the significance of the word void?"*). Verify the AI fetches a response from Gemini and renders the chat bubbles correctly.
6. **Clear Settings:** Click the gear, click Clear, and verify it returns to Offline Mode.
