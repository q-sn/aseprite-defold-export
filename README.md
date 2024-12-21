# Asefold 


![](https://github.com/Kaiqgs/asefold/blob/main/assets/icon.png?raw=true)
Aseprite Defold integration

Check the [usage information](/USAGE.md) for further information.

## export

![](https://github.com/Kaiqgs/asefold/blob/main/assets/export_no_scripts.gif?raw=true)

### Bundle Extension

```
python3 ci/release.py
```

# Roadmap and known issues, ranked
- [X] bugfix: not exporting ping-pong
- [X] bugfix: not exporting reverse
- [X] none playback
- [X] bugfix: says that it's writing lua module, when its not writing, only warning
- [X] option: merge visible before export
- [X] #aaa repeat last export
- [X] #aab store preferences
- [X] #aaf verb/cancel style dialogs
- [X] #aae persist preferences on `/tmp/` upon closing and opening Aseprite  
- [X] #aag new option: supress success message
- [X] #aah change: sprite export file moved to a temporary path
- [X] #aac new button: clear preferences
- [X] #aad check for string match in Ase user_data, instead of equal operation
- [X] #aai show pingpong revers warning once
- [ ] get animation from: layers
- [ ] sheet type: packed
- [ ] sheet type: columns
- [ ] read collision from layer
- [ ] split main code file into multiple files (should I though?)
- [ ] use case gif: save stacked former
- [ ] use case gif: save horizontal, row, and vertical
- [ ] use case gif: generate lua module
- [ ] try localization
