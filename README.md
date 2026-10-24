# youtube-resume : un skill Claude Code pour résumer une vidéo YouTube

Tu donnes le lien d'une vidéo YouTube à Claude Code. Il lit les sous-titres et te prépare une note
complète : les idées, les outils cités, les étapes, les conseils. Ensuite, tu lui poses tes
questions sur la vidéo, même dans une autre conversation.

## Installation (2 minutes)

1. Installe yt-dlp, l'outil qui récupère les sous-titres :
   - Mac : `brew install yt-dlp`
   - Windows ou Linux : `pip install yt-dlp`
2. Crée le dossier du skill et mets-y le fichier `SKILL.md` :
   - Mac ou Linux : `~/.claude/skills/youtube-resume/SKILL.md`
   - Windows : `C:\Users\<toi>\.claude\skills\youtube-resume\SKILL.md`
3. Relance Claude Code.

## Utilisation

```
/youtube-resume https://www.youtube.com/watch?v=...
```

La note est enregistrée dans `~/.claude/youtube-resume/`. Pour une question plus tard :
« Dans la vidéo sur X, il disait quoi sur Y ? »

La vidéo doit avoir des sous-titres (automatiques ou non), en français ou en anglais.

Par Binôme IA, sur Instagram.
