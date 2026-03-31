# ViralCut AI

## Current State
Backend `generateScript` and `generateScenes` call placeholder `https://example.com/...` URLs that return errors. The video rendering step is a pure timer simulation. There is no real OpenAI integration and no way to configure an API key.

## Requested Changes (Diff)

### Add
- Admin-settable OpenAI API key stored in backend state
- `setOpenAIKey(key: Text)` admin-only backend function
- `hasOpenAIKey()` public query to check if key is configured
- Real OpenAI `gpt-4o` API calls in `generateScript` and `generateScenes` with structured JSON prompts
- Frontend API key settings panel (admin only) shown when key is missing
- Error handling in the frontend wizard when generation fails

### Modify
- `generateScript`: Replace example.com call with real OpenAI `/v1/chat/completions` HTTP outcall, returning JSON matching Script type
- `generateScenes`: Replace example.com call with real OpenAI outcall, returning JSON array matching Scene[] type
- Frontend wizard: Show meaningful error messages if generation fails; show config prompt if no API key set

### Remove
- Placeholder example.com HTTP outcall URLs

## Implementation Plan
1. Add `openAIKey` var and `setOpenAIKey` / `hasOpenAIKey` to backend
2. Build OpenAI HTTP outcall helper in backend for chat completions
3. Replace `generateScript` with real prompt → OpenAI → parse JSON Script
4. Replace `generateScenes` with real prompt → OpenAI → parse JSON Scene[]
5. Frontend: detect missing API key and show settings panel
6. Frontend: catch errors in generation steps and show toast
