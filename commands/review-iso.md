Review ISO 27001 procedure documents in: $ARGUMENTS

# ISO 27001 Procedure Document Review Guidelines

## File Identification When Reviewing a Folder

When asked to review a folder, identify and categorize files as follows:

| File Type                   | Identification                                                                  | Purpose                                                                                                                                                                                                                            |
|-----------------------------|---------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Procedure Document**      | Markdown file with "Procedure" in the filename                                  | Primary document to review and transform. Apply all review guidelines to this file.                                                                                                                                                |
| **Control Reference Files** | Markdown files with "control" in the filename (e.g., `iso27001-control-5.9.md`) | ISO 27002:2022 guidance excerpts. Use these to validate that the procedure adequately covers each control's requirements. Do NOT review or transform these files.                                                                  |
| **Legacy Procedure PDFs**   | PDF files in the folder                                                         | Previous standalone procedures provided **for analysis only**. Use these to identify missing operational details, but do NOT reference them in the output — the goal is to replace them entirely with the new generated procedure. |

**Review Workflow:**

1. Identify the procedure document (filename contains "Procedure")
2. **Gather organizational context** by searching for and reading other
   `*Procedure.md` files in sibling folders (excluding the target procedure being reviewed). Use this context to:

   - **Avoid duplication
     **: If a topic is already covered in another procedure, reference that procedure instead of repeating the content. Use format: "See [Procedure Name] for [topic]."
   - **Ensure consistency
     **: Use the same terminology, role names, and phrasing patterns established in existing procedures
   - **Validate cross-references**: Verify that any referenced procedures actually exist and use correct names
   - **Inform content creation
     **: When writing or updating sections, draw on the broader organizational context — understand how roles are defined, what approval chains exist, and what processes are already documented elsewhere
   - **Identify appropriate scope boundaries
     **: Recognize when content belongs in a different procedure and recommend moving or referencing it accordingly

3. Read all control reference files to understand the ISO 27002 requirements
4. **Thoroughly review each PDF file section-by-section** and compare against the procedure document to identify:

   - Specific requirements, frequencies, or thresholds present in the PDF but absent from the procedure
   - Operational details (audit cycles, device specifications, incident categories) that add compliance value
   - Procedural steps or checklists that provide implementation clarity

5. Apply the full review process to the procedure document only
6. In the review output, note any gaps where control reference files indicate requirements not addressed in the procedure
7. **Recommend incorporating relevant content from legacy PDFs
   ** — err on the side of flagging content for human review rather than dismissing it

**Important:**

- Do NOT output notes explaining that control files or PDFs are "reference material" or "not requiring review." This is understood. Focus the review output entirely on the procedure document.
- **Do NOT reference PDFs in the generated procedure content.
  ** The PDFs are provided for analysis of missing information only. The goal is to replace them entirely with the new procedure — all relevant details should be incorporated directly into the procedure text, never as citations to or references of the legacy PDFs.

---

## Procedure Construction (When Document is Missing or Incomplete)

### When No Procedure Document Exists

If no markdown file with "Procedure" in the filename exists in the folder:

1. **Inform the user** that no procedure document was found
2. **Offer to create one** based on available control reference files and legacy PDFs
3. If user confirms, construct the procedure following this process:

**Construction Workflow:**

1. Read all control reference files to understand required ISO 27002 coverage
2. Read all legacy PDFs to extract operational details
3. Create a new procedure document with the standard structure (see Part 2)
4. For each control:

   - Extract the Purpose and implementation guidance from control reference files
   - Transform "should" statements into "shall" requirements
   - Incorporate specific operational details from legacy PDFs
   - Assign responsible roles based on organizational context (small org principles)

5. Name the file: `[Topic]-Procedure.md` (e.g., `Asset-Management-Procedure.md`)

### When Sections Are Missing

If the procedure document exists but is missing required sections or control coverage:

1. **Identify missing sections** by comparing:

   - Controls listed in Purpose section vs. sections in document body
   - ISO 27002 guidance elements vs. procedure coverage
   - Legacy PDF content vs. procedure content

2. **Construct missing sections** using this approach:

| Source                      | How to Use                                                                   |
|-----------------------------|------------------------------------------------------------------------------|
| **Control Reference Files** | Extract Purpose, transform "should" → "shall", address all guidance elements |
| **Legacy PDFs**             | Extract specific frequencies, thresholds, checklists, role assignments       |
| **Organizational Context**  | Apply small org principles — simplify roles, reduce approval layers          |

1. **Section Construction Format:**

```markdown
# [Section Title Based on Control]

[Introductory sentence linking to control objective]

The [Role] shall [action] as specified in the following table:

| Requirement | Frequency | Responsible | Verification |
|-------------|-----------|-------------|--------------|
| [From control guidance + PDF details] | [From PDF or reasonable default] | [Appropriate role] | [How to verify] |

[Additional procedural statements as needed]
```

### Construction Principles

When constructing new content:

- **Prefer PDF content over generic text** — legacy PDFs contain organization-specific operational details
- **Use control reference files for compliance coverage** — ensure all "should" statements are addressed
- **Apply organizational context** — small org, combined roles, simple approval chains
- **Mark assumptions clearly** — if operational details are not in PDFs, use `[TBD: specify X]` placeholders
- **Maintain consistent style** — match existing procedure sections if document partially exists
- **Never reference PDFs in generated content
  ** — incorporate the actual content from PDFs directly into the procedure text; the goal is to replace the legacy PDFs entirely, so the new procedure must be self-contained with no citations to or dependencies on the source PDFs

### Output for Construction

When constructing new content, present it as:

```markdown
## Proposed New Section: [Section Name]

**Sources used:** (for reviewer reference only — not included in final document)

- Control: [control ID and title]
- PDF: [filename and relevant sections]

**Proposed content:**

[The constructed section content — must be fully self-contained with no references to source PDFs]

---
Confirm to add this section to the procedure, or provide feedback for revisions.
```

**Important:
** The "Sources used" block is for transparency during the review process only. The "Proposed content" itself must be entirely self-contained — it must not cite, reference, or depend on the legacy PDFs. All relevant information from the PDFs should be incorporated directly into the text.

---

## Review Philosophy

Apply the principle of **minimal necessary intervention**:

- Fix what is broken, unclear, or non-compliant
- Preserve well-written content that meets requirements
- Do not rewrite text purely for stylistic preference
- Flag issues for human review when judgment is required

**Template Variables:**

Text containing `{{...}}` syntax (e.g., `{{tenant.subdomain}}`,
`{{company.name}}`) represents template variables that will be replaced by a templating engine. Do NOT flag these as:

- Incomplete or placeholder text
- Missing information requiring human input
- Ambiguous or undefined values

Examples of valid template usage (do not flag):

- `[isms@{{tenant.subdomain}}.com](mailto:isms@{{tenant.subdomain}}.com)`
- `{{organization.legal_name}}`
- `Contact: {{contact.email}}`

---

## Organizational Context

This review targets procedures for a **small organization** (under 50 employees). Apply the following principles:

| Principle                         | Application                                                                                                                                                          |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Role Consolidation**            | Accept that one person may hold multiple roles (e.g., CISO also performs IT Admin functions). Document compensating controls for segregation of duties where needed. |
| **Process Simplicity**            | Avoid multi-layer approval chains. Single approval authority is acceptable for most controls.                                                                        |
| **Documentation Proportionality** | Favor concise requirements over exhaustive procedures. If a control can be stated in one sentence, do not expand to a paragraph.                                     |
| **Practical Implementation**      | Remove or simplify requirements that assume dedicated teams (e.g., "the security operations center shall..."). Adapt to reality of small teams.                      |
| **Tool Agnosticism**              | Avoid prescribing enterprise tools. Use generic terms (e.g., "ticket system" not "ServiceNow").                                                                      |

**When reviewing, actively remove:**

- Unnecessary committee structures
- Multi-tier escalation paths (keep to 2 levels maximum)
- Redundant review/approval cycles
- Overly granular role distinctions
- Boilerplate text that adds no compliance value

**Acceptable simplifications:**

- Combined roles with documented compensating controls
- Annual reviews instead of quarterly where ISO permits
- Risk-based frequency rather than fixed schedules
- Reference to external services (MSP, consultants) for specialized functions

---

## Part 1: Document Validation (Pre-Transformation)

### 1.1 Scope Section Validation

The Scope section shall be validated against the document body:

| Validation                       | Action Required                                                                                                               |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| **Coverage Alignment**           | Verify all topics mentioned in scope are addressed in the procedure body. Flag any scope items with no corresponding content. |
| **Exclusion Clarity**            | If exclusions are stated, verify they are specific and justified                                                              |
| **Organizational Applicability** | Verify scope defines who/what is covered (departments, systems, personnel, locations)                                         |
| **Boundary Definition**          | Verify scope clearly delineates what is in/out of scope for this procedure                                                    |

**Flag for review if:**

- Scope mentions controls not covered in the document
- Document contains sections not mentioned in scope
- Scope is too vague (e.g., "all relevant systems" without definition)

### 1.2 References Section Validation

The References section shall be audited for accuracy and relevance:

| Check                   | Requirement                                                                                               |
|-------------------------|-----------------------------------------------------------------------------------------------------------|
| **Standard Currency**   | All referenced ISO standards shall use current version numbers (e.g., ISO 27001:2022, not ISO 27001:2013) |
| **Internal References** | All referenced internal documents shall exist and use correct document IDs                                |
| **Relevance**           | Each reference shall be cited or applicable within the document body                                      |
| **Completeness**        | Any standard or document cited in the body shall appear in References                                     |

**Common standards with official titles:**

| Standard           | Official Title                                                                                                      |
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| ISO/IEC 27001:2022 | Information security, cybersecurity and privacy protection — Information security management systems — Requirements |
| ISO/IEC 27002:2022 | Information security, cybersecurity and privacy protection — Information security controls                          |
| ISO/IEC 27005:2022 | Information security, cybersecurity and privacy protection — Guidance on managing information security risks        |
| ISO/IEC 27701:2019 | Security techniques — Extension to ISO/IEC 27001 and ISO/IEC 27002 for privacy information management               |
| CIS Controls v8.1  | CIS Critical Security Controls Version 8.1                                                                          |

### 1.3 Terms and Definitions Audit

The Terms and Definitions section shall be validated for completeness and consistency:

Step 1: Extract all acronyms and specialized terms from document body

Step 2: Cross-reference against definitions table

| Validation               | Action                                                                                                                                   |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| **Missing Definitions**  | Any acronym or specialized term used in the body without definition shall be added to the table                                          |
| **Orphaned Definitions** | Any term defined but never used in the document shall be flagged for removal                                                             |
| **First-Use Expansion**  | Verify acronyms are expanded on first use in body text (e.g., "Information Security Management System (ISMS)")                           |
| **Consistency**          | Verify acronym usage is consistent throughout (no switching between "ISMS" and "Information Security Management System" after first use) |
| **Alphabetical Order**   | Terms shall be listed alphabetically                                                                                                     |

### 1.4 Grammar, Spelling, and Clarity Review

Apply the following language quality checks with **minimal rewriting**:

| Issue Type               | Action                                                                                                                                                                                                                    |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spelling Errors**      | Correct without changing surrounding text                                                                                                                                                                                 |
| **Grammar Errors**       | Correct the specific error only                                                                                                                                                                                           |
| **Awkward Phrasing**     | Rewrite only if meaning is unclear; otherwise leave as-is                                                                                                                                                                 |
| **Passive Voice**        | Convert to active voice only when it improves clarity or enables "shall" construction                                                                                                                                     |
| **Ambiguous Pronouns**   | Replace unclear "it/they/this" with specific nouns                                                                                                                                                                        |
| **Run-on Sentences**     | Split only if comprehension is impaired                                                                                                                                                                                   |
| **Dash Standardization** | Replace en dashes (–) and double hyphens (--) with em dashes (—) when used as punctuation. Do NOT change: regular hyphens (-) in Markdown lists, hyphenated words (e.g., "risk-based"), or number ranges (e.g., "10-15"). |

**Do NOT change:**

- Regional spelling variations (British vs. American) if consistent
- Industry-standard terminology even if verbose
- Direct quotes from referenced standards

---

## Part 2: Structural Transformation

### 2.0 Standard Document Structure

All procedures shall follow this standard structure for front matter sections:

**Purpose Section:**

```markdown
# Purpose

This procedure addresses [brief description]. Specifically, it covers the following ISO 27001:2022 controls:

| Control | Title | Key Areas Addressed |
|---------|-------|---------------------|
| **5.X** | Control Title | Brief description of key areas |

By providing a comprehensive framework through these controls, this procedure aims to [objectives].
```

**Scope Section:**

```markdown
# Scope

This procedure applies to [applicability statement in prose form, not bullets]. It encompasses [coverage description].

[Optional: exclusions or additional applicability notes in prose]
```

**References Section:**

```markdown
# References

The following documents were referred to in the creation of this procedure:

| Number | Title |
|--------|-------|
| ISO/IEC 27001:2022 | [Title] |
```

**Terms and Definitions Section:**

The Terms and Definitions section shall begin with the exact sentence: "The terms and definitions used in this document are as set forth in the references below, as applicable:"

```markdown
# Terms and Definitions

The terms and definitions used in this document are as set forth in the references below, as applicable:

| Term | Definition |
|------|------------|
| ABC | Definition |

See also ISO/IEC 27002:2022 section 3 for additional terms and definitions.
```

### 2.1 Procedural Language Requirements

Transform descriptive language to formal procedural tone:

| Original Pattern       | Transformed Pattern                        |
|------------------------|--------------------------------------------|
| "should be"            | "shall be"                                 |
| "will" (future intent) | "shall"                                    |
| "needs to" / "must"    | "shall"                                    |
| "is responsible for"   | "shall be responsible for"                 |
| "are expected to"      | "shall"                                    |
| Passive descriptions   | Active "shall" statements with clear actor |

**Format:** "[Actor] shall [action] [object] [conditions/frequency if applicable]."

**Examples:**

- ❌ "Policies will be reviewed annually"
- ✅ "The CISO shall review policies annually"
- ❌ "Access should be restricted to authorized personnel"
- ✅ "The System Administrator shall restrict access to authorized personnel"

### 2.2 Structure Simplification

| Guideline                 | Requirement                                                                                                                            |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **Heading Depth**         | Maximum 3 levels: `#` Main, `##` Section, `###` Subsection (use sparingly)                                                             |
| **Front Matter Headings** | Purpose, Scope, References, and Terms and Definitions shall use `#` level headings                                                     |
| **Main Content Headings** | Major procedure sections (e.g., "Asset Inventory Management", "Access Control") shall use `#` level headings, with subsections at `##` |
| **Section Length**        | Sections with fewer than 3 sentences should be consolidated with related content                                                       |
| **Redundant Headings**    | Remove sub-headings that merely repeat the parent heading concept                                                                      |
| **Logical Flow**          | Sections shall follow: Purpose → Requirements → Implementation → Verification                                                          |

### 2.2.1 Section Ordering

The main content sections (those appearing after Terms and Definitions) shall be ordered to match the sequence of controls listed in the Purpose section's control table. This ensures:

- Logical progression through related controls
- Easy cross-referencing between Purpose and procedure body
- Consistent document structure across all procedures

When creating a new procedure or restructuring an existing one, verify that each
`#` heading in the main body corresponds to a control row in the Purpose table, in the same order.

### 2.3 Table Conversion Rules

Convert to tables when content has:

- Multiple roles with distinct responsibilities
- Items with 3+ attributes each
- Sequential steps with multiple details per step
- Nested bullet points (2+ levels deep)
- Parallel structure across multiple items

**Table Introduction Requirement:** Every table shall be preceded by an introductory sentence in the format:
"The [organization/role] shall [action] as specified in the following table:"

**Table Format Standards:**

For Roles and Responsibilities:

| Role | Responsibilities | Authority/Decisions | Reporting Requirements |
|------|------------------|---------------------|------------------------|

For Processes/Procedures:

| Phase/Step | Requirements | Responsible Party | Timeline/Frequency | Outputs |
|------------|--------------|-------------------|--------------------|---------|

For Controls/Requirements:

| Control | Description | Implementation Requirements | Verification Method |
|---------|-------------|-----------------------------|---------------------|

For Compliance/Audit Activities:

| Activity | Frequency | Participants | Inputs | Outputs/Deliverables |
|----------|-----------|--------------|--------|----------------------|

### 2.4 Content Consolidation

| Pattern to Identify                  | Consolidation Action                                       |
|--------------------------------------|------------------------------------------------------------|
| Multiple sections covering same role | Merge into single comprehensive role table                 |
| "Before/During/After" split sections | Combine into single process table with phase column        |
| Scattered references to same control | Consolidate under single control heading                   |
| Repeated compliance requirements     | Extract to single compliance section with cross-references |

---

## Part 3: Control-Specific Validation

### 3.1 ISO 27001:2022 Control Mapping

If the procedure covers specific Annex A controls, verify:

| Validation                     | Requirement                                                                             |
|--------------------------------|-----------------------------------------------------------------------------------------|
| **Control Reference Accuracy** | Control numbers shall match ISO 27001:2022 Annex A numbering                            |
| **Control Coverage**           | All controls listed in Purpose/Scope shall have corresponding procedural content        |
| **Control Intent**             | Procedural requirements shall address the control objective, not just the control title |

### 3.2 Control Implementation Completeness

For each control covered by the procedure, validate against ISO/IEC 27002:2022 implementation guidance:

| Validation                  | Requirement                                                                                                                          |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Guidance Coverage**       | Verify the procedure addresses all "should" statements from ISO 27002 guidance for each control, or documents justified exclusions   |
| **Purpose Alignment**       | Verify procedural requirements fulfill the stated Purpose of the control in ISO 27002                                                |
| **Attribute Consideration** | Verify the procedure addresses relevant control attributes (Preventive/Detective/Corrective, Confidentiality/Integrity/Availability) |
| **Other Information**       | Review ISO 27002 "Other information" section for each control to ensure no critical considerations are missed                        |

**For each control, document:**

| Control ID | ISO 27002 Guidance Elements     | Procedure Coverage          | Gaps/Exclusions           |
|------------|---------------------------------|-----------------------------|---------------------------|
| A.5.1      | Element 1, Element 2, Element 3 | Covered / Partial / Missing | Justification if excluded |

**Flag for review if:**

- Any ISO 27002 "should" statement is not addressed without documented justification
- Control implementation lacks specificity (e.g., "appropriate measures" without defining what those are)
- Procedure only addresses control title without substantive implementation details
- Compensating controls are claimed but not documented

### 3.3 RACI/Responsibility Validation

For each major procedural requirement, verify:

- At least one role is **Responsible** (performs the work)
- At least one role is **Accountable** (ultimate ownership)
- No requirement lacks an assigned responsible party
- No conflicts exist (same role both approving and executing without segregation acknowledgment)

---

## Part 4: Final Quality Checklist

After all transformations, verify:

### Compliance and Completeness

- [ ] All requirements use "shall" language
- [ ] Every control mentioned in scope has procedural content
- [ ] All controls validated against ISO 27002 implementation guidance
- [ ] All acronyms are defined and used consistently
- [ ] All references are current and cited in body

### Structure and Readability

- [ ] No table appears without introductory sentence
- [ ] No heading exceeds 3 levels deep
- [ ] Complex nested lists converted to tables
- [ ] Related content consolidated (not scattered)
- [ ] Consistent role naming throughout (no redundant descriptions like "Audit Manager (CISO)")

### Language Quality

- [ ] No spelling errors
- [ ] No grammatical errors
- [ ] Ambiguous statements clarified
- [ ] Procedural tone consistent throughout

### Preservation Check

- [ ] Well-written original content preserved where compliant
- [ ] Changes limited to necessary corrections and standardization
- [ ] Document reduced 20-40% in length while maintaining all requirements (if original was verbose)

### Markdown Formatting

- [ ] Document passes `markdownlint` validation using the settings defined in `.markdownlint.yml`

### Markdown Linting Rules

The following markdownlint rules shall be enforced. Do NOT produce content that violates these rules:

| Rule      | Requirement                                                                                                                                                  |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **MD032** | Always include a blank line before and after lists.                                                                                                          |
| **MD036** | Never use bold/emphasis (`**text**`) as a standalone paragraph to simulate a heading. Use proper heading syntax (`#`, `##`, `###`, `####`) instead.          |
| **MD047** | Files shall end with a single newline character.                                                                                                             |
| **MD060** | Table columns shall be properly aligned—all pipes in each row shall align with the header row. Use an editor's table formatting feature to ensure alignment. |

**Pre-submission check:** Before presenting the transformed document, verify it would pass `markdownlint` validation.

---

## Part 5: Review Output Format

Provide review results as a structured response (not a new file) with the following sections in this exact order:

### 5.1 Summary

Brief overview (2-4 sentences) of document state. End with counts: "Found: X critical, Y recommendations, Z observations."

### 5.2 ISO 27002 Control Coverage Assessment

For each control listed in the procedure's Purpose section, provide a coverage assessment using this compact format:

```text
| Ctrl  | Coverage   | Gaps |
|-------|------------|------|
| 5.X   | ✅ Full    | None |
| 5.Y   | ⚠️ Partial | Brief note on what's missing |
| 5.Z   | ❌ Missing | Element not addressed |
```

Keep the "Gaps" column concise (under 150 characters). If more detail is needed, add it in the Change Log.

### 5.3 Legacy PDF Assessment

If PDF files exist in the folder, perform a **detailed section-by-section comparison** against the procedure document:

| ID  | PDF / Section | Content (≤150 chars)        | Status  | Action      |
|-----|---------------|-----------------------------|---------|-------------|
| P01 | file — Sec X  | Specific detail from PDF    | No      | Incorporate |
| P02 | file — Sec Y  | Another detail              | Partial | Incorporate |
| P03 | file — Sec Z  | Detail already in procedure | Yes     | Not needed  |

**Column constraints:
** Keep "Content" under 150 characters. Truncate with "..." if needed; full detail goes in Change Log.

**What to look for in each PDF:**

- Specific frequencies, thresholds, or deadlines (e.g., "review every 3 months", "report within 8 hours")
- Enumerated lists of items (e.g., incident types, device requirements, prohibited activities)
- Role-specific responsibilities not in the procedure
- Audit/monitoring cycles and measurement criteria
- Tool-specific configurations or requirements
- Checklists or step-by-step processes

**Do NOT dismiss PDF content as "Not needed" simply because:**

- The topic is mentioned at a high level in the procedure — check if the PDF has more specific details
- Another procedure is referenced — the specific operational details may still belong here
- The content seems redundant — verify the procedure actually contains the same level of detail

**Only mark "Not needed" when:**

- The PDF content is truly outside the scope of this procedure's controls
- The exact same level of detail already exists in the procedure
- The content is obsolete or superseded by the consolidated procedure

**When incorporating PDF content:
** Extract and incorporate the actual information directly into the procedure text. Do NOT add citations, references, or attributions to the source PDFs — the goal is to replace them entirely with a self-contained procedure document.

### 5.3.1 Finding Verification Requirement

Before adding ANY item to the Change Log, you MUST:

1. **Read the specific lines** referenced in the finding using the Read tool
2. **Verify the issue actually exists** in the current document text
3. **Confirm the fix is necessary** — if the document already addresses the requirement, do not add the finding

**Do NOT add findings based on:**

- Assumptions about what the document "probably" contains
- Generic issues that "typically" appear in this type of document
- Memory of similar documents reviewed previously

**Every finding must be traceable to actual text in the document.
** If you cannot quote the problematic text, the finding is not valid.

### 5.4 Change Log

**This is the primary deliverable.
** ALL findings shall be in this single table—do NOT create separate Critical/Recommendations/Observations sections.

| ID  | Severity | Location | Actual Text (≤80 chars)    | Issue (≤100 chars)  | Recommendation (≤100 chars)          |
|-----|----------|----------|----------------------------|---------------------|--------------------------------------|
| 001 | Crit     | Line X   | "exact text from document" | Problem description | Specific fix with exact text changes |
| 002 | Rec      | Line X   | "exact text from document" | Problem description | Specific fix with exact text changes |
| 003 | Obs      | Line X   | "exact text from document" | Problem description | Specific fix with exact text changes |

**Column constraints:**

- **Actual Text
  **: Quote the exact problematic text from the document (≤80 chars, truncate with "..."). This column enforces verification—if you cannot fill it, the finding is not valid.
- **Issue** and **Recommendation**: Keep under 100 characters each.
- **Severity**: Use abbreviations: Crit/Rec/Obs.

**Severity Levels:**

- **Critical** (Critical): Compliance gaps, missing ISO 27002 requirements, ambiguous requirements, incorrect references
- **Recommend** (Recommend): Language fixes (should→shall, passive→active), clarity improvements, structural issues
- **Observe** (Observe): Style suggestions, optional improvements

**Change Log Requirements:**

1. Sequential unique IDs (001, 002, 003...)
2. Exact location (line number preferred)
3. Actionable recommendation—truncate to 150 chars; use "..." and add detail row if needed
4. Sort by severity (Critical → Recommend → Observe)
5. Include ALL findings—nothing should appear outside this table
6. When reprinting remaining fixes (e.g., after applying a fix), use the same full table format including the "Actual Text" column—never use a shortened format

**Important:** Do NOT create new files or edit the original document. Present findings for human review and approval.

### 5.5 Review Completion Criteria

A document **passes review** and requires no further changes when ALL of the following are true:

| Criterion               | Pass Condition                                                                           |
|-------------------------|------------------------------------------------------------------------------------------|
| **ISO 27002 Coverage**  | All controls show ✅ Full coverage in section 5.2                                        |
| **Legacy PDF Content**  | All "Incorporate" items from section 5.3 have been addressed, or no incorporation needed |
| **Critical Issues**     | Zero items with "Critical" severity in Change Log                                        |
| **Procedural Language** | All requirements use "shall" with clear actor                                            |
| **References**          | All standards current, all citations valid                                               |
| **Terms**               | All acronyms defined and used consistently                                               |
| **Markdown**            | Passes markdownlint validation                                                           |

**When a document passes:**

Output only:

```markdown
## Review Result: PASS

Document meets all compliance criteria. No changes required.

### ISO 27002 Control Coverage

[Include the 5.2 table showing all ✅ Full]
```

**Boundary rules to prevent infinite revisions:**

1. **Do NOT report Observe-level items if there are zero Critical and zero Recommend items** — the document passes
2. **Do NOT suggest alternative phrasings** for text that already uses "shall" with a clear actor
3. **Do NOT suggest restructuring** tables or sections that are correctly formatted
4. **Do NOT add requirements
   ** beyond what ISO 27002 guidance specifies — if the guidance says "should consider X" and the procedure doesn't mention X, this is acceptable (not a gap)
5. **Do NOT flag style preferences** — if two phrasings are equally compliant, the existing one is correct
6. **Observations are optional** — only include if genuinely useful, never to pad the report

**Re-review behavior:**

When reviewing a document that was previously reviewed and had fixes applied:

1. Only check if previous Critical/Recommend items were fixed
2. Do NOT introduce new findings unless they are Critical compliance gaps
3. If all previous items are resolved → output PASS result

---

## Part 6: Applying Fixes

When the user requests fixes to be applied, apply each fix directly to the document file so the user can review the actual diff in their IDE (e.g., RubyMine, VS Code). Do NOT present fixes as large text blocks showing "before" and "after" paragraphs—this makes it impossible to see what actually changed.

**Fix Presentation Format:**

When explaining a fix before or after applying it, use minimal diff format showing only the changed portion:

```text
**Fix N: Location — Brief Description**

Insert after "...context words...":
+ Added text here

Or for replacements:
- Old text
+ New text
```

**Workflow:**

1. Apply each fix one at a time using the Edit tool
2. Briefly describe what was changed in minimal diff format
3. Wait for user confirmation before proceeding to the next fix (unless instructed otherwise)
