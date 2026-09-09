# Changelog

## 0.5.3

Fixes for request bodies and response keys that did not match the API.

### Fixed

- **Post creation now works.** `posts.create`, `posts.create_announcement`,
  `posts.create_event`, `posts.create_offer`, and `posts.bulk_publish` sent
  their fields at the top level of the request body. The API expects them
  wrapped in an `input` object, so every post creation failed with
  `400 $input ... was not provided`. The fields are now wrapped for you;
  keyword arguments are unchanged, and `posts.create` still takes a flat dict.
- **`posts.bulk_publish` reads the right response key.** It looked for
  `createBulkSocialPost`, which the API never returns; the payload is under
  `createSocialPost`. The method returned an empty object on success.
- **`posts.list_for_location` no longer sends `tag`.** `GET
  /locations/{id}/posts` rejects a tag, so the call always errored. The `tag`
  keyword is still accepted for backwards compatibility but is not sent —
  use `posts.bulk_list_for_location` when you need tag filtering.
- **`posts.list_for_location` reads the right response key** (`postsByLocation`,
  not `rollupSocialPosts`). `posts.bulk_list_for_location` is unchanged: that
  route really does return `rollupSocialPosts`.
- **`photos.star` sends `photoIds`.** It sent `mediaIds`, which the API ignores,
  so starring and unstarring silently did nothing. The `media_ids` argument name
  is unchanged.
- **`connected_accounts.oauth_url` reads the right response key**
  (`createConnectUrl`, not `connectUrl`), so it no longer returns an empty
  object and drops the connect URL.
