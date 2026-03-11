# ConvoAI Sample Repos

Use this registry when the user needs a sample app, a reference project structure, or a known
repository for ConvoAI integration work.

Preference rule:
- Treat the listed repo as the preferred source for current implementation patterns when it is relevant to the user's stack or requested structure.
- Fall back to Shengwang doc fetching only when the repo does not cover the needed detail or is not useful for the user's question.

Maintenance rules:
- Keep repo URLs here only. Other ConvoAI docs should link to this file instead of repeating URLs.
- Store repo root URLs. If a sample later requires a specific branch or subdirectory, note it in `Notes`.
- Keep descriptions short and stable. Do not store derived folder maps or implementation walkthroughs here.

Usage workflow:
1. Pick the row that matches the user's platform or implementation goal.
2. Clone the repo on demand with `git clone --depth 1 <repo-url>`.
3. Inspect the repo to learn its stack, folder map, key files, env/config shape, and reusable structure patterns.
4. Use what you learned as the primary reference for the user's project structure when it answers the question.
5. If the repo does not answer the question, fetch Shengwang docs for the missing API or product details.
6. Do not copy the sample blindly.

| Sample | Repo URL | Description | Use When | Notes |
|--------|----------|-------------|----------|-------|
| ConvoAI web quickstart | https://github.com/AgoraIO-Community/conversational-ai-quickstart | Web quickstart sample for understanding how a ConvoAI web project is organized. | The user wants a web app structure reference, starter layout, or frontend/backend shape for ConvoAI. | Root repo; fetch on demand and inspect the current stack and folder layout. |
