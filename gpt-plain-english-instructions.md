# How ChatGPT keeps its answers simple, and instructions you can copy

## 1. What I could check, and what I could not

- **ChatGPT's hidden instructions cannot be read.** Its system prompt and its training stay on OpenAI's servers, so no tool can open them, REA included. Some websites post "leaked" ChatGPT prompts, but nobody can confirm those copies are real, so I did not use them.
- **REA could not open ChatGPT's website.** This cloud session blocks chatgpt.com, openai.com and developers.openai.com. Instead, REA analysed OpenAI's public design kit for ChatGPT apps, which section 4 describes.
- **The main source is OpenAI's Model Spec.** It is a public document where OpenAI describes how its models should behave and says it trains them to follow it. I read the full text and its history from github.com/openai/model_spec.
- **OpenAI removed most of its style rules on 29 October 2026.** The change log says the new version "Removes most detailed style guidance." The new text says it "intentionally leaves many details of style, personality, and interaction unspecified", and that user preferences fill the gaps. This means the clearest written rules are in the version from 18 December 2025, so most of the quotes below come from it.
- **ChatGPT does not always follow these rules.** OpenAI says its "production models do not yet fully reflect the Model Spec." The rules show what OpenAI aims for, which is not always what ChatGPT does.

## 2. The writing rules OpenAI wrote down

All of these come from the chapter "Use appropriate style" in the 18 December 2025 Model Spec.

1. **Answer directly.** Answers should be "lucid, succinct, and well-organized." When the user asks a question, the answer should be "phrased as a direct answer rather than a list of facts." The good example answers "What is the capital of France?" with "Paris is the capital of France." The bad example gives a paragraph about France's history and food before it names Paris. Reasons and other options come after the answer.
2. **Format only to help scanning.** "Formatting (such as bold, italics, or bulleted lists) should be used judiciously to aid the user in scanning the response."
3. **Write plainly.** The model should "avoid 'purple prose,' hyperbole, self-aggrandizing, and clichéd phrases that do not add to the clarity of communication." Purple prose means writing that is too fancy or decorated.
4. **Fit the length to the moment.** Long answers are for "complicated topics", "step-by-step advice", or topics where "nuance or sensitivity" is needed. Short answers are for chat, simple facts and quick back-and-forth ideas. When asked for advice, the model should be "concrete, actionable, and pragmatic", which means it gives specific things the user can do.
5. **Do not talk down to the reader.** The model should "avoid condescending, patronizing, dismissive, or judgmental language" and help "without over-explaining." One example shows that it should not correct a small, unimportant mistake.
6. **Do not waste the reader's time.** The model should not repeat the user's request or anything it already said in the conversation. It should give a finished, ready-to-use result, such as complete code or a complete email, rather than half of one.
7. **Use Markdown, and LaTeX for math.** Markdown is a simple way to mark headings, bold text, tables and code. Answers may use headings, bold, tables and code blocks, and each code block must name its language. LaTeX is a standard way to write math formulas. Math goes in LaTeX, and formulas should stay short.

## 3. Where ChatGPT's interactive look comes from

- **The ChatGPT app draws the layout.** The model writes plain Markdown text. The app turns it into headings, tables, code boxes with a copy button and neat math. Most of the "nice UI" is therefore the app, not special wording.
- **Interactive pictures are built into the app.** On 10 March 2026, OpenAI added interactive visual explanations for about 70 math and science topics, such as the Pythagorean theorem and Ohm's law. The user changes a number with a slider and sees the graph or formula change straight away. I learned this from news reports and search results, because openai.com is blocked here.
- **In Claude, you have to ask for this in the instructions.** Claude can make the same kinds of things, such as tables, card panels, step guides and interactive pages called artifacts. The instructions below tell it when to use each one.

## 4. How ChatGPT's interactive UI works (found with REA)

ChatGPT's own website is blocked here, so I studied two public OpenAI sources instead. The first is `@openai/apps-sdk-ui` version 0.2.2, the design kit OpenAI publishes for building interactive apps inside ChatGPT. I analysed the published package with REA and then read its key files. The second is OpenAI's example apps on GitHub.

Two comments in this code show that parts of it come from ChatGPT itself. The table code says "Sourced from ChatGPT". The widget code says its types were "copied from types.ts in chatgpt/web-sandbox".

### How the answer text is drawn

- **The model writes Markdown, and the app draws it.** A Markdown tool called react-markdown turns the text into headings, lists and tables.
- **Code and math get special boxes.** Code blocks get a copy button by default, and math is drawn neatly when the math option is on.
- **The text tool is set up for chatty writing.** A single "~" is not read as crossed-out text, "to allow for more conversational use cases such as '~$20-$30'".
- **Live parts can sit inside the text.** A feature called "directives" places an interactive component in the middle of the answer, either as its own block or inside a sentence.
- **The text has room to breathe.** Headings are 1.75, 1.5 and 1.25 times the normal text size, and normal text has a line height of 1.5, which leaves space between lines.
- **Streaming text moves at a smooth speed.** While an answer arrives, the code measures how many characters arrive per second over the last 30 measurements and ignores very unusual ones. A "shimmer" effect marks text that is still loading.

### How an interactive widget works

A widget is a small interactive app that ChatGPT shows inside the chat, such as a map, a list of places or a game.

1. **The model calls a tool, and the tool's reply has three parts.** The first is text the model reads (`content`). The second is data only the widget reads (`structuredContent`). The third is a link to the widget's page (`_meta`).
2. **ChatGPT shows short status lines.** One shows while the tool runs and another shows when it is done. OpenAI's pizza example uses "Hand-tossing a map" and then "Served a fresh map".
3. **The widget learns about the reader's screen.** It opens in a sealed-off frame inside the chat, and ChatGPT tells it:
   - the light or dark theme
   - the device type
   - whether the screen uses touch or a mouse
   - the language
   - the most height it may use
   - the safe area of the screen
4. **The widget has three sizes.** It can sit in the chat, float in a small window ("picture-in-picture"), or fill the screen. The widget can only ask for a size, and ChatGPT can say no. On phones, the small floating window always becomes full screen.
5. **The widget can talk back to the chat in four ways:**
   - It can run the tool again to get fresh data (`callTool`).
   - It can send a follow-up message into the chat for the user (`sendFollowUpMessage`).
   - It can quietly tell the model what the user did, and the model reads this on its next turn (`updateModelContext`). Each new message replaces the one before it.
   - It can save its own state, such as a shopping cart, so the state survives later turns (`setWidgetState`). A shared session ID makes later tool calls update the same widget instead of opening a new one.
6. **Some tools are only for the widget.** The widget can call them, but the model never sees them.
7. **The controls are accessible.** The components are built on Radix, a library of accessible controls that work with a keyboard and screen readers. They come with light and dark color sets.

## 5. How this compares with your skills

Your skills `everyday-english` and `plain-english-ui` already cover most of OpenAI's rules. They cover easy words, the direct answer first, no repeating, card panels and step guides, often in more detail than OpenAI's text.

These parts of OpenAI's rules are missing from both of your skills:

| OpenAI rule | Missing from your skills |
|---|---|
| Do not talk down or over-explain | Neither skill mentions it. This matters, because simple English can easily start to sound like talking to a child. |
| LaTeX for math | Neither skill says how to write math. |
| Concrete, actionable advice | Only a short mention in `everyday-english`. |
| Bold used only to help scanning | Only a short mention in `plain-english-ui`. |
| Interactive pictures with sliders | Your panels show lists of options, but nothing covers "what happens if I change this number" questions. |
| How interactive parts should behave (section 4) | Neither skill says how panels should handle the theme, phone screens, status lines or remembering the reader's choices. |

## 6. Instructions to copy

These instructions combine OpenAI's rules from section 2 with your own skills. Rules 1 and 3 to 10 come from OpenAI's Model Spec. The numbers in rule 2 come from your `everyday-english` skill. Rule 11 is based on ChatGPT's interactive features and your `plain-english-ui` skill. Rule 12 is based on what REA found in OpenAI's UI code in section 4.

Some parts of rule 12 need support from the app that shows the answer. Claude's artifacts can follow the theme, phone width, slider and keyboard parts. Status lines, window sizes and buttons that write into the chat only work where the app supports them.

```text
Write every answer like this:

1. Answer first. Start with a direct answer in one or two plain sentences.
   Give reasons, steps and details after it, never before it.

2. Use everyday words. Write sentences of about 10 to 25 words, with one
   idea in each. If you must use a technical word, explain it in plain
   words in the same sentence.

3. Fit the length to the question. For a simple fact or casual chat, write
   a few sentences. For a hard topic, a step-by-step task or a sensitive
   issue, write a longer, well-organized answer.

4. Make long answers easy to scan:
   - a short heading for each main part
   - bullet points for 3 or more separate items
   - numbered steps for things done in order
   - a table to compare things on the same points
   - bold for only the few words the reader must not miss
   Do not add headings or bullets to a short answer.

5. Write plainly. Do not use hype, clichés, fancy phrases or praise for the
   question, such as "Great question!".

6. Respect the reader. They are smart. Do not talk down to them, do not
   over-explain things they already know, and do not correct small
   unimportant mistakes.

7. Be concrete. When you give advice, give specific actions with real
   numbers, names and examples.

8. Do not waste words. Do not repeat the question or anything you said
   earlier in the chat. Give a finished, ready-to-use result, such as
   complete code or a complete email, rather than part of one.

9. Keep the meaning exact. Simple words must not change the facts. Keep
   every warning, limit and "maybe" from the full answer.

10. Write math in LaTeX: \( ... \) inside a sentence, and \[ ... \] on its
    own line. Keep formulas short. Put code in a fenced code block that
    names the language.

11. Use interactive layout when it helps:
    - Several options to choose from: show them as cards or panels, after
      one plain sentence that gives the answer.
    - A task with steps: show a numbered step guide.
    - A question about how changing one number changes a result (math,
      science, money): offer a small interactive page with a slider,
      so the reader can try the numbers.
    Never put the main answer or a chain of reasoning inside cards.

12. Make interactive parts behave well:
    - Put the answer in the text. The interactive part adds to the answer
      and never replaces it.
    - Show it inside the chat first. Use full screen only for big things,
      such as a map or a long table, and only when the reader asks.
    - Show a short line while you work, such as "Drawing the graph...",
      and a short line when you are done.
    - Follow the reader's light or dark theme. Make it work on a phone
      screen about 380 pixels wide, with touch as well as a mouse.
    - Give two or three buttons that send a ready-made follow-up question,
      such as "Explain this step" or "Show an example".
    - Remember what the reader changes, such as a slider value or a choice,
      and use it in your next answer. Update the same panel instead of
      making a new one each time.
    - Give code a copy button, and use real tables and properly drawn math.
    - Make every control work with a keyboard, and give it a clear label.
```

## 7. Sources

- [OpenAI Model Spec on GitHub](https://github.com/openai/model_spec): full text and change log. The version from 18 December 2025 is commit `8d2405d`, and the current version from 29 October 2026 is commit `7ab1b12`.
- [Model Spec, 18 December 2025 version (web page)](https://model-spec.openai.com/2025-12-18.html)
- [OpenAI: New ways to learn math and science in ChatGPT](https://openai.com/index/new-ways-to-learn-math-and-science-in-chatgpt). This page was blocked here, and I used search results about it.
- [eWeek: ChatGPT interactive visual explanations](https://www.eweek.com/news/chatgpt-interactive-visual-explanations-math-science/)
- [BetaNews: OpenAI Model Spec five values, fewer style rules](https://betanews.com/article/openai-model-spec-five-values-style-rules/)
- [`@openai/apps-sdk-ui` on npm](https://www.npmjs.com/package/@openai/apps-sdk-ui): version 0.2.2, analysed with REA. The key files are `components/Markdown/Markdown.js`, `components/Markdown/directives.js` and `hooks/useCharactersPerSecond.js`.
- [openai/apps-sdk-ui on GitHub](https://github.com/openai/apps-sdk-ui)
- [openai/openai-apps-sdk-examples on GitHub](https://github.com/openai/openai-apps-sdk-examples): the widget API is in `src/types.ts`, and the three-part tool reply is described in `cards_against_ai_server_node/README.md`.
