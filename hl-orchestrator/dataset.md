Each of the 158 records captures a full problem:
  the question with optional preamble context, a hint about the expected answer format, a list of intermediate steps showing the reasoning trace, a final ground-truth answer, an answer type (symbolic LaTeX, numeric,
  or textual — plus some structured JSON outputs), and meta fields recording the original author, year, and   source document.
  
 Problems are grouped by `id`, `q_id`, and `sub_id`, so multi-part questions (a, b, c...) stay linked as progressive derivations. For example, `id` would refer to the question source (which worksheet), `q_id` would correspond to e.g. "question 1" and `sub_id` to "question 1 part a". 

  The scientific content spans the core LWS curriculum: solar wind and cosmic-ray transport (Parker equation, termination shock, stochastic acceleration), heliospheric and magnetospheric dynamics, and
  ionospheric E/F-region physics. The content is entirely drawn from peer-taught graduate level coursework.

  The dataset was built by taking the original LWS problem sets and normalizing them into a uniform JSONL schema suitable for LLM evaluation — preserving the pedagogical step-by-step solutions while adding
  machine-checkable fields (answer type, format hints, ground truth). This structure supports a companion programmatic grader that evaluates model responses with unit-aware numerical tolerance, symbolic equivalence checking, and schema validation, so the benchmark tests genuine physical reasoning (correct units, valid derivations, properly formatted outputs) rather than surface string matches.

  Full details can be found in the paper.