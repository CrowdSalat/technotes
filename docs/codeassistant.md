# Setup local ai code assistent

Integrate you local ai code assistent into your IDE with [Continue](https://github.com/continuedev/continue). Use a local model server like Ollama or InstructLab to host a [granite code model](https://github.com/ibm-granite/granite-code-models) which can be used by Continue. 



```shell
## use ollama

# intall
brew install ollama
# run ollama server
ollama serve


# pull granite model in other terminal 
# see list of models (and tags): https://ollama.com/library
ollama pull granite-code:20b # ~11GB
ollama pull granite-code:8b # ~4,6GB
ollama pull granite3-dense:2b # ~1,6GB


# install continue in vscode
code --install-extension continue.continue
```

To use it in vscode Continue extensions edit `code $HOME/.continue/config.json` or use or use ui of extension in vscode to add new sessions and select ollama with auto detect.

```yaml
  "models": [
    {
      "title": "Granite Code 20b",
      "provider": "ollama",
      "model": "granite-code:20b"
    }
  ],
  ...

"tabAutocompleteModel": {
    "title": "Granite Code 8b",
    "provider": "ollama",
    "model": "granite-code:8b"
  },
```

# notes on downloaded models and disk size

## ollama

LLM are saved here: `/usr/share/ollama/.ollama/model` or `~/.ollama/models`
list downloaded models: `ollama list`
remove model from disk: `ollama rm MODEL`

## instruct lab

LLM are saved here: ~/. cache/instructlab/models/ 
