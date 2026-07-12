# Installing First Principles Thinking Skill for Codex

Run these commands to install the skill:

```bash
git clone https://github.com/swapnildahiphale/first-principles-thinking-skill.git ~/.codex/first-principles-thinking
mkdir -p ~/.agents/skills
ln -s ~/.codex/first-principles-thinking ~/.agents/skills/first-principles-thinking
```

## Windows

Use a junction instead of a symlink:

```powershell
git clone https://github.com/swapnildahiphale/first-principles-thinking-skill.git "$env:USERPROFILE\.codex\first-principles-thinking"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\first-principles-thinking" "$env:USERPROFILE\.codex\first-principles-thinking"
```

## Verify

Restart Codex, then ask a design question like "Should I use microservices or a monolith?" The skill should activate automatically.

## Updating

```bash
cd ~/.codex/first-principles-thinking && git pull
```

## Uninstalling

```bash
rm ~/.agents/skills/first-principles-thinking
rm -rf ~/.codex/first-principles-thinking
```
