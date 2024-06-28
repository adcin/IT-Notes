# Logseq - an open source knowledge management tool.
A privacy-first, open-source platform for knowledge management and collaboration. \*quote from [github repo](https://github.com/logseq/logseq/blob/master/README.md)

- [Home Page](https://logseq.com/)
- [Documentation](https://docs.logseq.com/)
- [Roadmap](https://trello.com/b/8txSM12G/roadmap)
- [Blog](https://blog.logseq.com/)

# Config

My system: Linux, fedora 40, KDE 6.1 , Logseq 0.10.9 installed as AppImage

- Location of global config: `~/.logseq`
- Location of cache etc.: `~/.config/Logseq`

# Troubleshooting

##### Logseq not starting

\1. Rename the cache Folder:
```
mv ~/.config/Logseq ~/.config/Logseq.bkup
```

\2. Start Logseq
_Logseq should now open with your basic config but only the demo graph. You need to index your graph folder and everything should be fine again._

\3. Remove the backup cache folder:
```
rm -r ~/.config/Logseq.bkup
```

