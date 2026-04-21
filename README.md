This project conducts a follow-up analysis of MindScape [ 22], an
AI-supported journaling system that combines mobile sensing with
large language models to promote student well-being. Building on
the original feasibility study, we examine how contextual (Weeks
1-6) and generic (Weeks 7-8) prompts influence four behavioral do-
mains: physical activity, social interaction, digital habits, and sleep,
using per-user week labeling to account for staggered start dates.


MindScape_IEEE_version:

-- task2.ipynb: merge the data with week labels and behavior domain category
-- task3_1_fixed.ipynb — extract signals with z-score normalization (all 3 windows), Computes signal_before, signal_after, signal_change, behavioral_improvement
using Z-SCORE NORMALIZATION before averaging features into a composite.
Outputs:
  prompts_with_signals_task3_1day.csv
  prompts_with_signals_task3_3day.csv
  prompts_with_signals_task3_7day.csv

-- task4.ipynb: Classifies each unique journal prompt as REFLECTIVE or REDIRECTIVE
using the Groq API (llama-3.3-70b-versatile).

-- task5.2.ipynb: Classifies journal responses to REDIRECTIVE prompts as INTENTION or REFLECTION
using the Groq API (llama-3.3-70b-versatile).

-- textual_analysis_v2.ipynb: Linguistic analysis of improved vs non-improved journal entries.

-- debugging.ipynb: Response type classification by LLM prompt


-- signal_analysis2_fixed.ipynb: Unit of analysis: one row = one journal entry for one domain.
Improvement flag: behavioral_improvement (yes/no) from task3_1_fixed output,
which uses z-score normalized composite signal with domain-specific direction.
