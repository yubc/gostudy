
1.用户代码片段设置
  ```"defer func by ficow": {
		"prefix": "idefer",
		"body": [
			"defer func() {\n\t$1\n}()",
			"$2"
		],
		"description": "defer func by ficow"
	},
	"func by ficow": {
		"prefix": "ifunc",
		"body": [
			"func $1() {\n\t$2\n}",
			"$3"
		],
		"description": "func by ficow"
	},
	"multiple imports": {
		"prefix": "ims",
		"body": "import (\n\t\"${1:package}\"\n)"
	},
```

2.设置
```
{
    "[go]": {
        "editor.insertSpaces": true
    },
    "editor.tabSize": 2,
    "files.associations": {
        "*.vue": "vue"
    },
    "eslint.autoFixOnSave": true,
    "eslint.options": {
        "extensions": [
            ".js",
            ".vue"
        ]
    },
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "vue",
        "vue-html"
    ],
    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/dist": true
    },
    "emmet.syntaxProfiles": {
        "javascript": "jsx",
        "vue": "html",
        "vue-html": "html"
    },
    "extensions.autoUpdate": true,
    "editor.renderWhitespace": "boundary",
    "editor.cursorBlinking": "smooth",
    "workbench.welcome.enabled": false,
    "window.zoomLevel": 0,
    "editor.fontSize": 16,
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 5000,
    "go.buildOnSave": true,
    "go.lintOnSave": true,
    "go.vetOnSave": true,
    "go.buildTags": "",
    "go.buildFlags": [],
    "go.lintFlags": [],
    "go.vetFlags": [
        "-all"
    ],
    "go.coverOnSave": false,
    "go.formatOnSave": true,
    "go.formatTool": "goreturns",
    "go.useCodeSnippetsOnFunctionSuggest": true,
    "go.autocompleteUnimportedPackages": true,
    "go.goroot": "D:/Go",
    "go.gopath": "E:/workspace/rrms.com;e:/gotest",
    "go.gocodeAutoBuild": false,
    "vsicons.dontShowNewVersionMessage": true,
    "telemetry.enableTelemetry": false,
    "window.menuBarVisibility": "default",
    "workbench.sideBar.location": "left",
    "extensions.ignoreRecommendations": true,
    "workbench.iconTheme": "vscode-icons",
    "vetur.format.styleInitialIndent": true,
    "vetur.format.scriptInitialIndent": true,
    "workbench.colorTheme": "Monokai",

    "markdown.styles": [
        "D:/Program Files (x86)/Microsoft VS Code/resources/app/extensions/markdown/media/vscode-markdown-css/markdown-github.css"
    ],
   
    // Extension - vscode-pandoc
    "pandoc.htmlOptString": "-s -f markdown_github -t html5 -H D:/Program Files (x86)/Microsoft VS Code/resources/app/extensions/markdown/media/vscode-markdown-css/markdown-github-pandoc.html"

}
```

3.插件

|名称|作用|
|:------|:------|
|auto close tag||
|auto rename tag||
|auto-open markdown preview||
|code runner||
|color highlight||
|debugger for chrome||
|eslint||
|font-awesome codes for html||
|git history||
|go||
|html css support||
|html-less-class-completion||
|htmlhint||
|javascript(es6) code snipets||
|lua||
|markdownlint||
|npm||
|npm intellisense||
|output colorizer||
|prettify json||
|start git-bash||
|version lens||
|vetur||
|vscode-i18n||
|vscode-icons||
|vscode-proto3||
|vuehelper||
|yaml||
|yarn||

