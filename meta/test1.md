```
https://www.facebook.com/stories/115428148280792/UzpfSVNDOjI3NzIzMDE1MjIwNjM4Njk4/?bucket_count=9&source=story_tray
```

Decoding the Base64 Story ID:

UzpfSVNDOjI3NzIzMDE1MjIwNjM4Njk4
→ S:ISC:277230152206386 98  (approximately)

How to Test This Properly

```
1. Log in as User A (victim), create a private story
2. Note the story URL structure
3. Log in as User B (attacker) or log out entirely
4. Substitute the story ID and attempt access
5. Try the Graph API equivalent:
   GET graph.facebook.com/v19.0/{story_id}?fields=id,owner,media&access_token=...
```

