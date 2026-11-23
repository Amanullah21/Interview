## How to Explain Your Project to an Interviewer (60–90 Seconds)

Use a tight **Problem → Architecture → Tough Engineering Details → Reliability/Results** flow instead of reading off your resume line by line:

1. **The hook & flow (15s):** "I built a real-time, voice-enabled AI interview platform using Next.js 16, TypeScript, and Material UI. It walks candidates through four stages—interview setup, hardware diagnostics, a live voice interview session, and a detailed AI performance scorecard."


2. **Voice & frontend engineering (25s):** "On the browser side, I wired together the Web Speech API and Web Audio API (`SpeechSynthesis`, `webkitSpeechRecognition`, `AudioContext`, and `AnalyserNode`) so users can talk naturally, interrupt the AI mid-response, edit live transcripts, and avoid speaker-to-mic echo loops."


3. **Evaluation & reliability (30s):** "For grading, I built a 100-point rubric system across seven assessment dimensions tailored to technical, coding, behavioral, scenario, and intro interviews using Gemini structured outputs. Because LLM calls can fail, lag, or return malformed data, I added deterministic fallbacks, IP rate limiting, and a searchable session audit dashboard."


4. **Wrap-up (10s):** "So the main focus wasn't just wrapping a chat prompt—it was making real-time voice interaction smooth and keeping scoring reliable in production."



---

### 10 AI Project Interview Questions & Answers

**1. Walk me through the end-to-end flow of your AI Interview platform.**

* It starts with a configuration step (role/type), moves into hardware diagnostics to verify mic/audio readiness before the session starts, runs the live voice interview with real-time transcription and speech output, and ends with an AI scorecard generated from the session. Separating setup and hardware checks upfront prevents mid-interview permission or audio failures.



**2. Why use browser-native Web Speech and Web Audio APIs instead of a third-party voice SDK?**

* Browser-native APIs (`SpeechSynthesis`, `webkitSpeechRecognition`, `AudioContext`, `AnalyserNode`) keep setup lightweight with zero extra SDK overhead and let me inspect/control audio nodes and speech states directly. The trade-off is browser support quirks—for example, `webkitSpeechRecognition` varies across browsers—so pre-session hardware checks and graceful fallbacks matter.



**3. How did you handle voice barge-in (the user interrupting the AI while it is speaking)?**

* When the user starts speaking while `SpeechSynthesis` is active, I cancel/stop the ongoing speech output immediately and route the incoming speech into the active recognition/turn flow. Without barge-in, voice interviews feel rigid and unnatural because the user has to wait passively until the AI finishes talking.



**4. How do you prevent echo or the AI picking up its own voice through the microphone?**

* When the speaker plays AI audio near an active mic, feedback/echo can corrupt transcripts or trigger false turns. Combining multi-layer acoustic echo suppression with turn/state awareness (muting/gating or filtering self-triggered input during playback) keeps the transcript clean.



**5. Why allow users to edit live transcripts during or after voice recognition?**

* Speech-to-text makes mistakes—especially on technical terms, acronyms, names, or accented speech. Letting users fix transcript errors gives them control over accuracy before evaluation runs, which prevents unfair scoring caused purely by a misheard word.

**6. How did you design the 100-point evaluation engine across 7 dimensions and multiple interview types?**

* Different interview formats—technical, coding, behavioral, scenario-based, and introductory—test different signals. Using category-specific rubrics mapped to a 100-point scale across 7 dimensions makes scoring consistent and explainable rather than giving a vague, arbitrary rating.



**7. Why use structured outputs (like Gemini structured outputs) instead of parsing free-text LLM responses?**

* Free-text responses break UI scorecards when formatting drifts or extra prose appears. Enforcing a strict schema with structured outputs ensures every score, dimension breakdown, and rubric field comes back in a predictable shape that the frontend and evaluation pipeline can safely consume.



**8. What are "deterministic evaluation fallbacks," and when do they trigger?**

* They are rule-based or deterministic scoring/fallback paths that kick in if the LLM API times out, rate-limits, errors out, or produces invalid/incomplete output. They guarantee the candidate still gets a valid, consistent result instead of a broken screen or stalled evaluation.



**9. How did you protect the app against abuse, spam, or runaway API costs?**

* AI and speech endpoints can get expensive or spammed quickly, so I implemented IP-based rate limiting to throttle excessive requests and protect backend/LLM usage. In production, I would pair IP limits with auth/user quotas and server-side validation so limits cannot be bypassed from the client.



**10. What does the session audit dashboard do, and how does it help debug real sessions?**

* It gives a searchable view of interview sessions and their evaluation records so you can trace what happened during a run. When a user questions a score or an edge case occurs, having session traceability makes it much easier to inspect inputs, outputs, and outcomes instead of guessing.



---

Want to do a quick spoken practice run where I ask one of these 10 questions and give feedback on how you answer it?
