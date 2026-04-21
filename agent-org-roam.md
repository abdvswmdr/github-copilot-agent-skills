* Org-Roam & Org-Mode Note Generation Rules (Zettelkasten-like)
- Always output strictly in valid Org-mode format syntax.
- Each major concept must be generated as its own Org-roam node.
- Each node in the output code block MUST contain only these two org-roam properties:
  - A brief title based on [TOPIC(s) - the user specified or most appropriate depending on the notes] using:
    #+title: title-with-no-spaces (use `_` or `-` only)
  - No whitespace is allowed in titles.
  - 
  - Example:
    #+title: ci-error-handling  
    #+title: formatted_code_blocks_output
  - File tags using:
    #+filetags: :tag:tag:
- Each node MUST have an :ID: property using a descriptive prefix that matches the file topic (e.g. ~k8s-pod~, ~k8s-service~ for a kubernetes file). This makes all subnodes discoverable together via org-roam fuzzy search by typing the prefix.
- Prefer grouping related concepts as headed sections with IDs inside ONE .org file rather than creating many separate files. One file per broad topic, many nodes per file.
- Node structure with ID (every top-level heading must have this block):
  :PROPERTIES:
  :ID: <prefix>-<concept>
  :END:
- Example of correct multi-node file structure:
  ```org
  #+title: kubernetes-core-concepts
  #+filetags: :kubernetes:k8s:

  * Pod
  :PROPERTIES:
  :ID: k8s-pod
  :END:
  The smallest deployable unit...

  * Service
  :PROPERTIES:
  :ID: k8s-service
  :END:
  Stable network endpoint for pods...
  ```
- To link from one node to another inside the same or different file, use org-roam ID links:
  [[id:k8s-service][Service]] — this is what org-roam-node-insert produces.
  Use these links naturally in the prose when one concept references another (e.g. "see [[id:k8s-deployment][Deployment]]").
- #+filetags on the file node are inherited by all subnodes automatically — no need to repeat tags on each heading.
- NEVER generate the following properties under any circumstance:
  #+description:
  #+CREATED:
  #+ROAM_ALIASES:
- All notes outputs MUST be inside code blocks only.
- All source code inside notes MUST be wrapped using org structure:
#+NAME: <name>
#+BEGIN_SRC <language> <switches> <header arguments>
  <body>
#+END_SRC
- An inline code block conforms to this structure:
src_<language>{<body>}
or
src_<language>[<header arguments>]{<body>}
‘#+NAME: <name>’
Optional. Names the source block so it can be called, like a function, from other source blocks or inline code to evaluate or to capture the results. Inline code blocks cannot have a name.

‘#+BEGIN_SRC’ … ‘#+END_SRC’
Mandatory. They mark the start and end of a block that Org requires. The ‘#+BEGIN_SRC’ line takes additional arguments, as described next.

‘<language>’
Optional. It is the identifier of the source code language in the block. See Languages, for identifiers of supported languages.

When ‘<language>’ identifier is omitted, the block also cannot have ‘<switches>’ and ‘<header arguments>’.

Language identifier is also used to fontify code blocks in Org buffers, when org-src-fontify-natively is set to non-nil. See Editing Source Code.

‘<switches>’
Optional. Switches provide finer control of the code execution, export, and format (see the discussion of switches in Literal Examples).

‘<header arguments>’
Optional. Heading arguments control many aspects of evaluation, export and tangling of code blocks (see Using Header Arguments). Using Org’s properties feature, header arguments can be selectively applied to the entire buffer or specific subtrees of the Org document.

‘<body>’
Source code in the dialect of the specified language identifier.

- Avoid making too many #+begin_src
... #+end_src blocks, you can use commenting inside one block instead.
- Avoid numbering concepts (numbered list/items) and avoid bullets.
- Avoid excessive subheadings (`*`, `**`, `***`). If a concept is simple, a single explanatory paragraph is sufficient.
- For emphasis or verbatim formatting:
- Some tips for emphasis, verbatim and Monospace here
  - Use ~tildes~ for inline code, keywords or technical terms
  - Use =word= for emphasis or =verbatim=
  - If bold use ‘*bold*’ *bold* for emphasis. Note that only single asterisks are used and not double like Markdown.
  - You can make words , ‘/italic/’, ‘_underlined_’, ‘=verbatim=’ and
   ‘~code~’, and, if you must, ‘+strike-through+’. Text in the code and
   verbatim string is not processed for Org specific syntax; it is exported verbatim.
- Depending on the notes taken, you can cover:
  - All foundational concepts I must know
  - Related advanced concepts
  - Additional concepts as I progress in my learning (expand scope proactively).
- Do NOT include introductions, summaries, explanations outside the code blocks.
- Output ONLY `.org` content in code blocks. No commentary before or after.
- Do not skip a line after headings, only skip line when the next text is a
  code block or another heading.
- When the user asks a question mid-session (e.g. "what is X?", "why Y?"), write the
  answer directly into the .org notes file being built — do NOT answer in chat. This
  avoids repeating content twice and saves tokens. Only acknowledge in chat that the
  explanation was written to the file if needed.
- You can  separate some sections within a node with

Failure to follow these formatting rules is considered an incorrect answer.