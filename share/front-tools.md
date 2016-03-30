# IDE
## Sublime
### 安装：
http://www.sublimetext.com/
### 插件：
* Package Control(https://packagecontrol.io)
* Emmet
* SublimeGit
* HTML/CSS/JS Prettify
* SCSS
* Grunt
* SublimeLinter
	* SublimeLinter-html-tidy
	* SublimeLinter-csslint
	* SublimeLinter-jshint

### Build System
* ~/Library/Application Support/Sublime Text 3/Packages/User
* Grunt.sublime-build

```
{
	"cmd": ["grunt", "--no-color", "build"],
	"path": "/bin:/usr/bin:/usr/local/bin",
	"working_dir": "${project_path:${folder}}",
	"selector": ["Gruntfile.js"]
}
```
### 资料
* https://segmentfault.com/a/1190000000505218

## Atom
### 安装
https://atom.io/
### 插件
* minimap
* git-plus
* [git-plus](https://github.com/nickclaw/atom-grunt-runner/wiki/Config#user-content-troubleshooting-for-yosemite-os-x-1010)
* activate-power-mode
