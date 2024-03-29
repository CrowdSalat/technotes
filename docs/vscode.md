# VS Code

## Debugging

Vscode expetcs a launch.json and a task.json file in .vscode directory in the root your selected vscode workspace. The preLaunchTask field in the launch.json refers to a label of a task in task.json. 

For debugging with firefox you need an [additional vscode extension](https://marketplace.visualstudio.com/items?itemName=firefox-devtools.vscode-firefox-debug). There is an official microsoft [recipes for debugging repo](https://github.com/microsoft/vscode-recipes/tree/master) on github.

## Remote SSH and SCP

[Official documentation](https://code.visualstudio.com/docs/remote/ssh)

- Install 'Remote - SSH' Extension
- Open config via the gear symbol which appears next to the 'ssh targets' batch when your mouse hovers over it over
- Edit this configureation text file
- Connect to it. First time vscode might  prompt two times for your password. (Once for the ssh login and once for the start of the vs code server)

Use vscode as usual. **Open Folder** and the **integrated terminal** will connect to the remote server not your local one.

One for each connection:
```
Host alias
    HostName hostname
    User user
    IdentityFile privateKey
```

trouble shooting:

- you can restart the vs code server on the remote server with prompt `> Remote-SSH: Kill VS Code SErver on Host...` (CTRL + SHIFT+P). This might be necessary if you add a user to a new group.

## useful extensions

- Spellchecker for txt and md: TODO maybe [LanguageTool Linter](https://github.com/davidlday/vscode-languagetool-linter)  
- Linters:
    - md: [markdownlint](https://github.com/DavidAnson/vscode-markdownlint)
    - [Bash script](https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck)
- Remote Connect
    - [SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
    - [Container](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
    - [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
- [Rainbow CSV](https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv) - highlight and queries
- [change-case](https://marketplace.visualstudio.com/items?itemName=wmaurer.change-case) - change between different cases (camel, snake etc.)
- [Render Linebreaks](https://marketplace.visualstudio.com/items?itemName=medo64.render-crlf)
