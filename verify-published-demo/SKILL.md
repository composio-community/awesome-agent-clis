---
name: verify-published-demo
description: Check whether a public direct MP4 URL serves the approved local export after a product demo or tutorial is replaced. Use for published-file verification and stale-video checks when you have the approved MP4.
---

# Verify a published product demo

Use the [Valmera demo verifier](https://github.com/ABO3SKRALMASAOODI/valmera-mcp/blob/b250a92b926fdcc6f00767b72e414fc6dbe6efa9/docs/evaluation/verify_published_demo.py). It runs with Python 3.9 or later and the standard library. Review the source at that revision and save the script locally, or use it from the repository checkout. The check needs no Valmera account, API key, or uploaded file.

Get the approved export from the user or their release artifacts and the exact public MP4 download URL to check. A hosted player page or a stream manifest needs a playback check instead.

```sh
python3 verify_published_demo.py --approved approved.mp4 'https://example.com/demos/current.mp4'
```

Read the JSON result and exit code.

| Result | Interpretation |
| --- | --- |
| `match` / 0 | The request received the approved bytes. Check the visible steps, chapter links, and captions in the viewer's player separately when relevant. |
| `different` / 1 | Confirm that this URL should serve the exact approved export. If a cache may be involved, preserve the first result and repeat once with `--revalidate`. If the host transcodes uploads, compare against the approved downloadable rendition. |
| `unavailable` / 2 | The checker could not obtain a complete direct MP4. Inspect the error before retrying or drawing a conclusion. |

Report the outcome for the URL and request tested. Do not infer that every CDN region or adaptive rendition serves the same version. The checker reads a local MP4 and issues a GET request. Follow the user's existing authorization before changing a live embed, file, or cache.

The [test fixture](https://github.com/ABO3SKRALMASAOODI/valmera-mcp/blob/b250a92b926fdcc6f00767b72e414fc6dbe6efa9/docs/REFRESH-PUBLISHED-PRODUCT-DEMOS.md#run-both-checks-on-the-recorded-demo) includes two playable clips. The approved export returns `match`; the older cut returns `different`.
