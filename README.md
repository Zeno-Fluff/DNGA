# DNGA - Discord-Notification-Github-Action.

Send clean Discord notifications when you push to `main` or other branches when you publish a new release.

## Features
- Push notifications with commit message and author
- Release alerts with tag, name, and link
- Uses GitHub Secrets to keep your webhook safe

## Setup (if you don't want to clone the repo)

1. Create a file named `.github/workflows/discord.yml`
2. Paste the following code into it (modify as you wish):

name: Discord Notification

on:
  push:
    branches:
      - main
  release:
    types: [published]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Send Discord message for push
        if: github.event_name == 'push'
        env:
          DISCORD_WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
        run: |
          curl -H "Content-Type: application/json" \
            -X POST \
            -d "{\"content\": \"name here-\n🛠️ New push to *main* by ${{ github.actor }}\n🔗 [View Commit](${{ github.event.head_commit.url }})\n📝 Message: ${{ github.event.head_commit.message }}\"}" \
            $DISCORD_WEBHOOK

      - name: Send Discord message for release
        if: github.event_name == 'release'
        env:
          DISCORD_WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
        run: |
          curl -H "Content-Type: application/json" \
            -X POST \
            -d "{\"content\": \"name here-\n🚀 New release published by ${{ github.actor }}\n🏷️ Tag: ${{ github.event.release.tag_name }}\n📝 Name: ${{ github.event.release.name }}\n🔗 [View Release](${{ github.event.release.html_url }})\"}" \
            $DISCORD_WEBHOOK

5. create a webhook on your discord server either on the web or desktop app.           
6. Go to your repo → Settings → Secrets → Actions.
7. Add a secret named `DISCORD_WEBHOOK` with your Discord webhook URL.

Example of what it should look like when you're done ( minus my logo and repo )
![Gitbot](https://github.com/user-attachments/assets/59c36d8c-4dc7-44cf-9850-24ab46d94617)
