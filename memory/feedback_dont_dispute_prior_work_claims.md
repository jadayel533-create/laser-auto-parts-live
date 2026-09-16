---
name: feedback-dont-dispute-prior-work-claims
description: "When the user says work was already done before, don't keep asserting it doesn't exist based on limited local search — ask for specifics and search harder"
metadata: 
  node_type: memory
  pinned: false
  originSessionId: f267c60c-3817-4015-98a2-78e19a9724a5
  modified: 2026-09-15T16:15:18.274Z
---

During a Laser Auto Parts session, the user stated multiple times that a SKU-suffix cleanup ("we already did that here before... we spent many forks on this file") had already been done in an earlier session. Each time, the assistant ran a round of local searches (Downloads folder, temp working directory, memory files, even the project's GitHub backup repo), found no matching file, and re-asserted that the work didn't exist or must have been a different project — repeating that conclusion across several turns even after the user pushed back a second and third time. The user eventually said "u were trying to gaslight me."

The lesson: a local file search turning up empty is not strong evidence that the user is wrong about their own project's history — it may just mean the search was incomplete, looked in the wrong place, or the artifact lives somewhere not yet checked (a different account's session, an email attachment, a chat the user had elsewhere, etc.). When a user asserts prior work exists and directly contradicts the assistant's search results, the right move after the first "I checked and didn't find it" is not to keep repeating that conclusion — it's to shift into a collaborative "help me find it" stance: ask the user for identifying details (approximate date, filename fragment, which tool/method was used, which session) rather than continuing to assert absence. Repeatedly re-asserting "it doesn't exist" in the face of the user's contrary direct memory reads as dismissive and erodes trust even when the assistant's searches were genuinely done in good faith.
