# Social media management SOP

Goal: manage TikTok / Instagram and other social accounts: publish content, manage comments, send and receive DMs, read performance data, and run the user's own TikTok Shop.

## Prerequisite: connected accounts

All social operations work on connected accounts. First call `list_social_accounts` to get the connected accounts. When you generate an authorization link, the only way to confirm the connection is `list_social_accounts` after the account holder completes authorization. Generating a link is not the same as a successful connection.

- No connected account and the user wants to work on Instagram / Facebook → call `get_instagram_auth_link` / `get_facebook_auth_link` respectively; only Instagram Business/Creator professional accounts are supported. Send the `auth_url` to a trusted account holder, and after they finish, check with `list_social_accounts`. One Facebook authorization may connect several Pages, so always use `list_social_accounts` to get the full account list. YouTube has no MCP authorization tool; it has to be connected on the Trenz website (https://www.trenz.ai)
- TikTok Shop shoppable posts → use `get_tiktok_shop_auth_link`; regular creator accounts, shop official accounts and marketing accounts all show up as `tiktok-shop-pc` after connecting. `type` only selects the regional authorization app: domestic (default, US), cross-border (US/BR/MX), mx-local (Mexico), local-jp (Japan), local-eu (DE/ES/FR/GB/IE/IT); it does not decide the account kind. After the holder completes authorization, confirm with `list_social_accounts(platforms=["tiktok-shop-pc"])`. If a valid connection already exists, reuse it
- No connected account and the target is a regular TikTok video account (no Shop shoppable posts) → **prefer the authorization link**; the QR code is a fallback used only when the user explicitly asks for it:
  1. Call `get_tiktok_auth_link` to get `auth_url`; send the link to the account holder (not necessarily the user in this conversation; cloud phones and agency setups are common), and tell them to open it in any browser, log in and confirm
  2. After the holder finishes, confirm with `list_social_accounts` that the account is visible. Account parameters of the later social tools take the numeric `id` from `list_social_accounts`, not the external string `platform_account_id`
  3. Only when the user explicitly says they want to scan a QR code, use `start_tiktok_qr_login`:
     a. First remind the user to open the TikTok app on their phone and be ready (the QR code is valid for a few minutes; being ready reduces code refreshes)
     b. Call `start_tiktok_qr_login`. **Show the QR code strictly following the decision ladder in the `hint` of the response**: the ladder details, render commands, validity, polling and expiry semantics all come from the hint, which is updated on the server and current on every call; do not work from memory or from this document. The hint requires you to start polling `check_tiktok_qr_login` as soon as you have the QR code; **do not wait for the user to confirm they see the code before polling**. A code that was never polled is treated as expired when the user actually scans it
     c. Core rule: **confirm the user actually sees the QR code before asking them to scan**. If no level of the ladder works, do not improvise (for example by reading out the QR content); guide the user to connect TikTok on the Trenz website (https://www.trenz.ai)
     d. After a successful connection, confirm with `list_social_accounts`, then continue the original operation
- Several accounts → confirm with the user which one to use before acting

## Publishing flow (publish_social_post, get_publish_status)

When asking for hashtag suggestions, the `keyword` of `suggest_hashtags` must be a single word with no whitespace, e.g. extract `bottle` from "water bottle"; do not pass a whole sentence, a phrase with spaces or several words, or the upstream returns 400.

New posts use `publish_social_post`. Prefer a structured object for its `platform_config`; the MCP serializes it before calling upstream. Older callers that already hold a raw upstream request can still pass a valid JSON object string. Publishing uses storage bucket fields, not URLs: video publishing only accepts bucket_name + bucket_key (see the upload_file tool description for the field mapping).

1. **Prepare the media (get the files into Trenz storage)**:
   - Local files: upload_file(file_name) to get a presigned upload URL → PUT upload with the shell (see the upload_file tool description for the command template) → note the returned bucket fields
   - AI-generated media: the result URL returned by check_task **cannot be published directly**; download it locally first, then go through upload_file + PUT as above
2. **Assemble the post**: bucket fields + video_title + publications (account, title/description, record tags, scheduled time); use media_items for multiple files/carousels. `publications[].tags` are only Trenz's internal record tags and do not become platform hashtags; hashtags to publish go into the actual platform title/description. For normal uploads, `media_items[].order` starts at **1**. Each account's `platform_config` is its own **structured object**; the MCP converts it into the upstream JSON string. The publishing route is decided by the account behind `platform_account_id`; adding `product_id` to a regular TikTok account does not turn it into a TikTok Shop shoppable post. TikTok Business video/photo location tags use a paired `location_id` + `location_name`; search first with `search_tiktok_locations` for the target account, never guess the ID. For the AI-generated content disclosure, choose `is_aigc` or `is_ai_generated` based on the publishing route; rules below
3. **Confirm before publishing**: show the user the full account, media, copy, publish time, location tag (if any) and any cost the current tool response can confirm. Submit only after explicit approval; publishing is an external action that cannot be undone
4. **Track the result for a limited time**: after `publish_social_post` returns a `session_token`, first check `skipped_accounts`; non-empty means some accounts were not accepted, and you must report them. Wait **3–5 seconds** before calling `get_publish_status`, and **3–5 seconds** between later calls; track automatically for at most **1 minute** from submission. Terminal session states are `completed / completed_with_errors / cancelled / failed`. `completed_with_errors` is a common mixed result for multi-account posts, so check every `publications[].status/error_message`; stop as soon as a terminal state is reached. If it is still `in_progress` after 1 minute, tell the user "The post has been submitted and the platform is still processing it; you can ask me to check again later", and do not keep polling or resubmit the publish request. Query the status separately only when the user later asks for it; after a successful publish, give the user the `platform_url` of each successful target. **Posts in progress do not appear in `list_published_posts`; do not use `list_published_posts` to judge whether something was published**

### TikTok publishing music strategy

First tell regular TikTok Business accounts apart from TikTok Shop accounts: both can pick music from a music library, but the search entry and the publishing ID differ. The Business music parameters below do not apply to TikTok Personal or Shop; the Shop video/photo flow is in the second half of this section. When one request publishes to several accounts, configure each `publications[]` target separately.

1. **Do not search candidate music by default**: if the user only asks to publish and has not brought up choosing, searching or reusing music, do not call `search_tiktok_music`, `list_recent_tiktok_music` or `search_tiktok_shop_music`, and do not give a long list of tracks for the user to decide on
2. **Recommend automatic music for photo posts by default**: when publishing a regular TikTok photo post and the user has not specified a song, recommend `{"auto_add_music":true}` in the publishing plan without calling the music query tools. This is a default suggestion awaiting confirmation; show it clearly in the final publishing list and never add it silently. Video publishing has no matching automatic-music default; if the user does not mention music, add no music settings
3. **Query only when the user explicitly asks for music**: confirm the target account first, then choose between recently used music (`list_recent_tiktok_music`) and music search (`search_tiktok_music`) per the current tool descriptions. Cost follows the current response. Trending music is market data; if the user has not given a country, confirm the target market first
4. **Let the user choose after the query**: show the song title, artist, duration, style and `preview_url`; do not pick for the user from the title alone. What goes into the publishing config is the returned `song_clip_id`, **not** `commercial_music_id`
5. **Assemble each publishing target's config**:
   - Video with a chosen song: `{"music_sound_id":"<song_clip_id>","music_sound_volume":80,"video_original_sound_volume":20}`
   - Optional trimming: `music_sound_start` / `music_sound_end` are both milliseconds and must not be below 0; when both are passed, end must be greater than start
   - `music_sound_volume` and `video_original_sound_volume` both range 0–100; music volume defaults to 100
   - Photo posts default to the recommended automatic music: `{"auto_add_music":true}`; after the user explicitly chooses a song, switch to `{"music_sound_id":"<song_clip_id>"}`; the two cannot be passed together
6. **Include it in the final confirmation**: besides account, media, copy and time, the publishing confirmation list must show the automatic-music switch, or the chosen song's title/artist, trim range, music volume and original sound volume. After the user confirms, put these fields directly into the `publications[].platform_config` object of `publish_social_post` and publish; the MCP handles serialization

Whether a post goes through the TikTok Shop shoppable route depends on the account type behind `platform_account_id`, not on whether `product_id` is included. When the target is a TikTok Shop account, the TikTok Business music parameters above, privacy, comment/duet/collection settings and `is_aigc` do not apply; a regular TikTok account does not switch to a shoppable post even if `product_id` is passed by mistake. The AI-generated content disclosure for shoppable videos uses `is_ai_generated`.

**TikTok Shop music library selection (video and photo)**: when the user asks for something like "pick a Lofi track from the music library", first confirm the target Shop account. Use `search_tiktok_shop_music`; cost follows the current tool description and response. Pass the target account and keyword per the current tool contract, show the returned songs' title, author, duration and preview link, and let the user choose. For more pages, keep the same account and keyword and pass the search ID and next-page cursor from the response. Check business errors and the actual song list; HTTP 200, an availability probe or an empty list does not prove a suitable song was found. Do not hard-code a region allowlist.

Write the chosen Shop `music[].id` into the same target's `publications[].platform_config.music_id`; do not use the Business `song_clip_id` / `music_sound_id`. Photo posts still need a complete `post_type` with the matching product link or shop anchor; `music_id` does not replace the shoppable config. Include the song and preview link in the final publishing confirmation; when the user has not mentioned music, do not search automatically or apply `auto_add_music` to Shop.

**Custom audio is a different request**: "upload a custom Lofi audio" is not the same as picking from the Shop music library. The current Shop photo publishing contract has no custom audio upload field, so a local audio path, URL or arbitrary ID cannot be used as `music_id`. You can explain the music library option to the user; if switching to a video with composed audio, confirm the change of content format first, and do not turn a photo post into a video on your own.

### TikTok Shop video proactive precheck

- `enable_precheck` only controls the **optional proactive precheck** before publishing a TikTok Shop shoppable video; default `false`
- Omitting the field or passing `false` skips the proactive precheck and goes straight to publishing; this does not bypass TikTok's final platform review
- Pass `true` only when the user explicitly asks for an extra precheck first; show the final value in the publishing confirmation list

### TikTok AI-generated content disclosure

- Regular video and photo posts from TikTok Business merchant-authorized accounts use `is_aigc`; the field works normally on this route
- When the target account is TikTok Shop and you publish a shoppable video, use `is_ai_generated`; do not also treat `is_aigc` as a valid disclosure field
- TikTok Shop shoppable photo posts currently do not support the AI-generated content disclosure, so `is_ai_generated` does not apply; other unsupported settings follow the TikTok account's defaults
- The pre-publish confirmation list must state clearly whether AI-generated content is disclosed, and the correct field must be written for the actual route

### TikTok publishing location tags

- Only supported for video and photo posts from TikTok Business merchant-authorized accounts
- `location_id` and `location_name` must go in pairs into the same account's `publications[].platform_config` object; the name must match the ID, and the location tag does not take effect if either is missing
- First query location candidates per the current `search_tiktok_locations` tool description, then let the user confirm by name/address. Use the ID and name from the same result as a pair; never guess an ID from a place name. Cost follows the current response
- When one request publishes to several accounts, each account can have its own location; the pre-publish confirmation list must show it per account

## Comment management (sync_comments, list_comments, reply_to_comment, delete_comment, set_comment_hidden)

- Comments are synced first, then queried: if no comments come back, run `sync_comments` first, then `list_comments`. `sync_comments` is asynchronous; a `list_comments` call right after it may return empty, so wait a few seconds and retry once before concluding "no comments"
- Pull a post's comments, group them by sentiment/topic, and give the user management suggestions
- Replying to comments is also an external action: before batch replies, give the user the list of replies to confirm
- Inappropriate comments are handled two ways: delete comments posted by the user's own account (`delete_comment`); **other people's comments cannot be deleted, so hide them (`set_comment_hidden`)** to make them invisible to others. Hiding can be undone at any time, deletion cannot; confirm each one with the user either way

## DM management (list_message_channels, list_conversations, list_messages, send_message)

The hierarchy is: channel (one connected social account, e.g. one WhatsApp number) → conversation (the chat with one contact/group) → message. To locate a target conversation, go `list_message_channels` → `list_conversations` → `list_messages` level by level.

- Typical scenarios: routing customer inquiries (filter conversations with the AI insight tags kol/b2b/buyer/fan), batch-checking unread messages, drafting replies
- **Sending a DM is an external action**: before sending, confirm the target conversation and message content with the user; when replying to several conversations, confirm one list at once
- Changing a conversation note (`set_conversation_remark`) on WhatsApp also rewrites the contact's display name on the other side. This is an externally visible change, so confirm the naming scheme with the user before batch labeling
- DM media does not use upload_file; use `get_message_media_upload_url` (the tool description has the field mapping)
- When a send fails because the send window has closed, tell the user plainly: the platform does not allow proactively messaging a conversation that has gone too long without a reply; it is not a fault
- **An empty channel list ≠ the user has no connected accounts**: DMs require the account authorization to include DM permission. Guide the user through each cause: accounts connected earlier may need to be reauthorized and reconnected; on TikTok only Business accounts support DMs (a personal account can switch to Business for free in the TikTok app, then reconnect the account); Instagram/Facebook are connected on the Trenz website (https://www.trenz.ai)

## Shop operations (get_seller_daily_report, get_seller_shops_overview, list_seller_products, get_seller_product)

The prerequisite is TikTok Shop **seller authorization** (separate from connecting a creator account), set up on the Trenz website (https://www.trenz.ai). `get_tiktok_shop_auth_link` connects the TikTok accounts that publish shoppable content (`tiktok-shop-pc`), including shop official accounts and marketing accounts; authorizing these accounts does not authorize the seller shop, and changing `type` cannot replace seller authorization. The MCP currently has no seller authorization tool.

- Typical scenarios: daily shop health check (`get_seller_daily_report` with yesterday's date), multi-shop comparison (`get_seller_shops_overview`), stock/slow-mover review (`list_seller_products` + `get_seller_product` for SKU stock)
- `get_seller_daily_report` "fetches a report", it does not "compute a report": when status=generating, wait 5–10 seconds and fetch again. **Every call counts toward the plan's usage limits**; once you have a ready result, reuse it and do not poll frequently
- **Staying in generating for a long time is a silent input error**, not slowness: stop and check whether the account is a seller account and the date is strictly YYYY-MM-DD; fix the input instead of retrying and burning usage
- When presenting the daily report, mind the data basis: the sales summary uses same-day real-time orders, and the business analysis uses T-2 data. It is normal for the two sets of numbers not to match; say so proactively
- Do not use the seller tools for product research; that is what search is for. These tools only cover the user's own shop

## Analytics (get_social_overview, get_social_account_trends, get_social_top_videos, get_social_top_products)

- Account level: follower growth, profile visits, overall engagement trend
- Content level: per-post views/likes/comments/shares; find the best-performing content patterns
- Present recommendations as a table plus one paragraph of conclusions (which content types are growing, what to post more of)

## Helper tools (hashtag, music, location and showcase tools)

Publishing helpers: `suggest_hashtags`, `search_tiktok_music`, `list_recent_tiktok_music`, `search_tiktok_shop_music` and `search_tiktok_locations`. Music query triggers and how they connect to publishing parameters follow the "TikTok publishing music strategy" above.

Creator product sourcing and showcase management: `search_affiliate_products` searches the affiliate marketplace, with commission rates in basis points (1000 = 10%); `list_showcase_products` lists the current shop/showcase; `add_showcase_products`, `remove_showcase_products` and `pin_showcase_products` actually change the creator's showcase, so before running them list the account, product IDs and target order for the user to confirm. A successful HTTP response from `add_showcase_products` can still carry per-product `errors`; `remove_showcase_products` / `pin_showcase_products` currently return no per-product result on success, so after calling them recheck with `list_showcase_products`, and never claim full success from an empty `data` alone.

## Notes

- Publishing, replying, deleting, disconnecting, and adding/removing/pinning showcase products all have external effects: **do not run them without the user's explicit confirmation**
- The access URLs returned by upload_file (signed_url) are for ai_generate reference media or user previews; publishing uses the bucket fields. Do not mix the two
- When the user wants "scheduled publishing", confirm whether the platform supports the parameter; if not, say so plainly and do not pretend it succeeded
