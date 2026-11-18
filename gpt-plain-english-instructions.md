# How ChatGPT keeps its answers simple, and instructions you can copy

## 1. What I could check, and what I could not

- **ChatGPT's hidden instructions cannot be read.** Its system prompt and its training stay on OpenAI's servers, so no tool can open them, REA included. Some websites post "leaked" ChatGPT prompts, but nobody can confirm those copies are real, so I did not use them.
- **REA could not open ChatGPT's website.** This cloud session blocks chatgpt.com, openai.com and developers.openai.com.
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

## 4. How this compares with your skills

Your skills `everyday-english` and `plain-english-ui` already cover most of OpenAI's rules. They cover easy words, the direct answer first, no repeating, card panels and step guides, often in more detail than OpenAI's text.

These parts of OpenAI's rules are missing from both of your skills:

| OpenAI rule | Missing from your skills |
|---|---|
| Do not talk down or over-explain | Neither skill mentions it. This matters, because simple English can easily start to sound like talking to a child. |
| LaTeX for math | Neither skill says how to write math. |
| Concrete, actionable advice | Only a short mention in `everyday-english`. |
| Bold used only to help scanning | Only a short mention in `plain-english-ui`. |
| Interactive pictures with sliders | Your panels show lists of options, but nothing covers "what happens if I change this number" questions. |

## 5. Instructions to copy

These instructions combine OpenAI's rules from section 2 with your own skills. Rules 1 and 3 to 10 come from OpenAI's Model Spec. The numbers in rule 2 come from your `everyday-english` skill. Rule 11 is based on ChatGPT's interactive features and your `plain-english-ui` skill.

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
```

## 6. Sources

- [OpenAI Model Spec on GitHub](https://github.com/openai/model_spec): full text and change log. The version from 18 December 2025 is commit `8d2405d`, and the current version from 29 October 2026 is commit `7ab1b12`.
- [Model Spec, 18 December 2025 version (web page)](https://model-spec.openai.com/2025-12-18.html)
- [OpenAI: New ways to learn math and science in ChatGPT](https://openai.com/index/new-ways-to-learn-math-and-science-in-chatgpt). This page was blocked here, and I used search results about it.
- [eWeek: ChatGPT interactive visual explanations](https://www.eweek.com/news/chatgpt-interactive-visual-explanations-math-science/)
- [BetaNews: OpenAI Model Spec five values, fewer style rules](https://betanews.com/article/openai-model-spec-five-values-style-rules/)
