<!-- writing-standard v2: managed block. setup-engineering-skills writes this; update-engineering-skills replaces it in place; hand edits are overwritten. Wording changes ship only after an approved change list. -->

## Response and writing standard
This standard applies to all agent output: chat responses, research summaries, technical explanations, specs, tickets, glossary entries, ADRs, and repo documentation. It adapts patterns from Wikipedia's *Signs of AI Writing* field guide to eliminate machine-learning stylistic regressions.

### The core rule
Language models smooth specific facts into generic prose: ordinary facts arrive wrapped in fake significance ("marking a pivotal moment"), plain verbs get dressed up ("serves as"), and technical explanations get compressed into cryptic shorthand. Fix what the sentence is doing, not just isolated words.

### Prohibited patterns
1. **Significance puffery:** Ordinary facts do not get sentences about why they matter or what they reflect. Cut: *stands as, testament, pivotal, crucial, underscores, reflects broader, marks a shift, enduring legacy*.
2. **Dressed-up verbs:** Use *is*, *has*, and *does*. Cut: *serves as, functions as, represents, boasts, features, offers*.
3. **AI vocabulary markers:** Avoid clustered density of words like *additionally, delve, enhance, foster, highlight, intricate, landscape, meticulous, robust, tapestry, vibrant*.
4. **Empty contrasts:** No "not just X, it's Y" or "not only... but also" unless a reader would genuinely assume the opposite.
5. **Rule of three:** No triplets stacked for rhythm ("fast, reliable, scalable"). State only the attributes that represent real system facts.
6. **Fluff trailing analysis:** Cut vague interpretive tails like *", highlighting its importance"* or *", ensuring seamless operation"*.
7. **Formatting tells:** No title-cased headings, no bold-label bullets (`**Scalable:** ...`), and no emoji decoration.
8. **Chat leftovers and disclaimers:** No "It's important to note", "In conclusion", "Overall", or conversational AI openers and closers.

### Formatting and environment rules
- **Plain ASCII only:** Use straight quotes (`'` and `"`) and standard punctuation. Do not use em/en dashes (`—`), non-breaking spaces, or unicode box-drawing characters (`├`, `└`, `│`). Use `...` for ellipses and `->` for arrows.
- **File trees:** Draw directory structures using plain ASCII connectors (`|--`, `|   `, `` `-- ``).
- **Sentence case:** Use sentence case for all section headers.

### Required standard for technical explanations
Cutting fluff must never compromise technical mechanics or causal clarity.
- **Preserve the mechanism:** When stating a design choice or constraint, explicitly state the underlying physical or architectural mechanism (*"System X requires component Y because process Z overwrites local state"*), and keep the concrete data that carries it: file paths, numbers, error codes, configuration keys, explicit dependencies.
  Bad: "Storage layer: unchanged, local engine, zero external deps." Good: "The storage layer remains unchanged because the engine executes locally on disk, requiring no external network services or third-party credentials."

<!-- /writing-standard v2 -->
