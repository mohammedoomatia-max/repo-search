# Public repository search

Search public GitHub repositories using the real GitHub Search API (`GET /search/repositories`, no token needed). Plain HTML, CSS and JavaScript, no build step. Open `index.html` or host it with GitHub Pages.

## The three states
| State | What it looks like | Role/announcement |
|---|---|---|
| **Loading** | Blue dashed box, spinner, text "Loading… Searching GitHub for “…”" | Announced politely via `aria-live` |
| **Error** | Red solid box, ✕ icon, "Request failed: …" with the cause and a **Try again** button | `role="alert"` |
| **Empty** | Yellow dotted box, ∅ icon, "No repositories found" | Announced politely via `aria-live` |

Each state differs in color, border style, icon, heading and wording, so none relies on color alone.

## Reaching each state without editing code
Open **"Reviewer: trigger each state"** under the search box:
- **Show loading state** runs the real search with a 5 second delay so the loading box stays visible.
- **Show error state** sends the request to an unreachable host, which produces a genuine network failure.
- **Show empty state** searches for a string no repository matches (`zzqx-no-such-repo-8f3k2`). The real API responds successfully with zero results.

You can also reach them naturally: search any word for results, search gibberish for empty, or go offline for the error.

## Error messages
Each says what failed and what to do:
- Network failure: couldn't reach GitHub, check your connection, try again.
- Rate limit (HTTP 403/429): unauthenticated search is limited to a few requests per minute, wait about a minute.
- Invalid query (422): shorten the search or remove special characters.
- Other server errors: usually temporary, try again.

## Empty is not an error
A response of `total_count: 0` means the request **succeeded**, so it is shown as a result ("No repositories found") with suggestions, never styled or worded as a failure.

## Other details
- A new search aborts the previous request, so stale results never overwrite newer ones.
- API text is inserted with `textContent`, never as HTML.
- Keyboard operable, with visible focus, light and dark themes, and reduced-motion support.
