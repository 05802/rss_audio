---
title:        "Nobel Prize lecture: Demis Hassabis, Nobel Prize in Chemistry 2024"
date:         2025-12-09 06:06:08 +0000
keywords:
- tag1
- tag2
mp3-url:      "https://pub-0eced6e15a5c47f7aa40f1e1dea66c92.r2.dev/2025-12-09-Nobel-Demis-Chemistry.mp3"
explicit:     "no"
block:        "no" # no means it is published
layout: podcast
excerpt_separator: <!--more-->
---
Dec 18, 2025 (date first published) 

# long description goes here 

<!--more-->

yyyy-mm-dd-short-descriptive-title.md

# how to find r2 public URL 

filename = yyyy-mm-dd-short-descriptive-title

# Verify upload exists
verify_output=$("$RCLONE" ls "${R2_REMOTE}:${R2_BUCKET}/${final_filename}" 2>&1)
if [[ $? -ne 0 ]]; then
    die "Upload verification failed. The file may not have uploaded correctly."
fi

# Generate public URL
# URL-encode the filename
encoded_filename=$(python3 -c "import urllib.parse; print(urllib.parse.quote('''$final_filename'''))" 2>/dev/null)
public_url="${R2_PUBLIC_URL}/${encoded_filename}"
