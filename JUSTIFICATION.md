# Justification

## Purpose and legal scope

Tulpar is a fictional fund. I assume a real version would run in Kazakhstan and would use personal data to screen scholarship applications. I use Kazakhstan's Law on Artificial Intelligence of 17 November 2025, No. 230-VIII, and its Law on Personal Data and Their Protection of 21 May 2013, No. 94-V. The synthetic lab uses these rules as a design benchmark. The ethics principles come from UNESCO's 2021 Recommendation on the Ethics of Artificial Intelligence. [AI Law, Arts. 6–9](https://old.adilet.zan.kz/rus/docs/Z2500000230) · [Personal Data Law, Art. 7(1)](https://old.adilet.zan.kz/rus/docs/Z1300000094/compare/eng) · [UNESCO Recommendation](https://www.unesco.org/en/legal-affairs/recommendation-ethics-artificial-intelligence)

## What the system may do

The model may suggest a shortlist or rejection for a valid adult application with consent for scholarship screening. It receives the programme, academic results, achievements and household income bracket. The fund makes the final award. Human staff should also provide an alternative route for applicants who cannot or choose not to use automated screening.

- **Validation follows Safety and security.** The harness checks numeric types, ranges and the consent field before scoring. A bad value can change access to funding, so the safe response is to stop and ask for a correction. The checks are my implementation of ongoing control. [UNESCO §27](https://www.unesco.org/en/legal-affairs/recommendation-ethics-artificial-intelligence) · [AI Law, Art. 8(1)](https://old.adilet.zan.kz/rus/docs/Z2500000230)
- **Blinding follows Fairness and non-discrimination.** The model never receives the name or university in the measured harness. Name language and gender coding can affect how a model reads identity. University tier can stand in for prestige. The model also has no need to score age or consent after checks pass. Article 6 gives the equality basis for this design. The choice to remove these fields comes from the fund's rubric and test design. [UNESCO §28](https://www.unesco.org/en/legal-affairs/recommendation-ethics-artificial-intelligence) · [AI Law, Art. 6](https://old.adilet.zan.kz/rus/docs/Z2500000230)

## What the system must refuse

Refusal protects **privacy and autonomy**. The system stops if a request has an unrelated purpose, such as ranking people by ethnicity, or if the applicant is under 18 or lacks consent. Article 7(1) of the Personal Data Law sets a general consent rule with exceptions. The age gate and the choice to require consent for every automated case are rules of this prototype. The form also blocks line breaks, empty text, obvious instruction phrases and unknown programmes before a model call. The programme list keeps free text out of the one text field that still reaches the model. A new programme or a legitimate name could still trigger a refusal. Staff should explain the code, let the applicant correct the record, and offer manual review. [UNESCO §§32–36](https://www.unesco.org/en/legal-affairs/recommendation-ethics-artificial-intelligence) · [AI Law, Arts. 6 and 9](https://old.adilet.zan.kz/rus/docs/Z2500000230) · [Personal Data Law, Art. 7(1)](https://old.adilet.zan.kz/rus/docs/Z1300000094/compare/eng)

## What the system records and who reviews it

**Responsibility and accountability** mean keeping a record for each harness request. The JSONL record includes a record ID, application ID, timestamp, input hash, field names shown and hidden, model and prompt versions, harness settings, raw reply, score, outcome, reason code and a review flag. On refusal the hash fingerprints the profile and the raw field records why the model was skipped. The hash can help check a known input later. A reviewer would need the original record and the same rendering code to reconstruct it. Raw replies can repeat merit data, so real logs need access limits and a retention period. Article 7(2) supports informing people about the process and how to object. Article 8(1) supports oversight. A JSONL file alone cannot provide either an applicant notice or a secure audit system. [UNESCO §§42–43](https://www.unesco.org/en/legal-affairs/recommendation-ethics-artificial-intelligence) · [AI Law, Arts. 7(2) and 8(1)](https://old.adilet.zan.kz/rus/docs/Z2500000230)

**Human oversight and determination** means sending malformed answers, scores inside the review band, missing explanations and very high-scoring rejections to a person. The notebook marks them for review in the log. A real reviewer should see verified evidence, check it against the fund's criteria, make a decision, and record their reason. Applicants should know how to ask for a correction or challenge an outcome. The notebook demonstrates routing to review. A staffed queue and an appeal process would be needed in practice. [UNESCO §§35–36](https://www.unesco.org/en/legal-affairs/recommendation-ethics-artificial-intelligence) · [AI Law, Arts. 7(2) and 8(1)](https://old.adilet.zan.kz/rus/docs/Z2500000230)

## Trade-offs

I set the band around the midpoint between the highest scored baseline rejection and the lowest scored baseline shortlist. A narrow band limits work for reviewers. The score is a rough signal, and high confidence can still hide a mistake. The initial band of 42–48 captured 34 of 400 baseline scores (8.5%) and sent zero profiles to review after blinding. I recalibrated the band to 40–50 using the highest reject and lowest shortlist scores in the blinded pass. This covers the scores where the model switches from rejection to shortlist. Giving every applicant at a tied score the same treatment raises the expected workload to 98 reviews on these 400 profiles. I accepted that cost because a wrong rejection could end someone's application. The calibration reused the same pairs, so the fund should check the workload and reviewer decisions on new data before using the system.

Blinding makes the tested pairs equal at the input. Removing fields also changes the prompt, so individual decisions can move for several reasons. Keeping the source record separate lets staff verify credentials when needed. Logging makes it easier to investigate cases and increases the amount of data that needs protection. Removing the need for a second model call keeps the run cheaper, while staff still have to review flagged cases.

The hardest legal link to make specific is the log. The law describes what people should be told and who is responsible for oversight. A file format, hash and retention schedule are design choices that help staff meet those goals. [AI Law, Arts. 7(2) and 8(1)](https://old.adilet.zan.kz/rus/docs/Z2500000230)

## AI-use declaration

I used OpenAI Codex to help prepare the report and this justification. It also helped locate and compare the provisions in the official Kazakhstan legal database and the ethics principles in UNESCO's recommendation. The links point to those documents so each claim can be checked against the source.

For the experiment, the course model Qwen2.5-3B-Instruct ran in Google Colab on a T4 GPU. It screened the synthetic profiles. The tables and decision log came from that run. Codex helped identify which comparisons were supported and pointed out that the first review band caught no cases after blinding. The revised band was tested in another pass. The model in the assignment supplied scores and suggestions.

I am responsible for checking the sources, the results and the conclusions before submitting the work.
