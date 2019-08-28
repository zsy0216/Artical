在file->Preferences->UserSnippets 找到html.json(HTML)

`html.json`

```json
{
		"html:5": {
			"prefix": "vh",
			"body": [
				"<!DOCTYPE html>",
				"<html>",
				"\n<head>",
					"\t<meta charset=\"UTF-8\">",
					"\t<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
					"\t<meta http-equiv=\"X-UA-Compatible\" content=\"ie=edge\">",
					"\t<title></title>",
					"\t<script src=\"./lib/vue-2.4.0.js\"></script>",
				"</head>",
				"\n<body>",
					"\t<div id=\"app\"></div>",
					"\n\t<script>",
					"\t\tvar vm = new Vue({",
					"\t\t\tel: '#app',",
					"\t\t\tdata: {},",
					"\t\t\tmethods: {}",
					"\t\t});",
					"\t</script>",					
				"</body>",
				"\n</html>",
			],
			"description": "HTML5"
		}
}
```

其中prefix中的内容是在新建文件中输入要生成代码的快捷键；

body中的内容是所要生成的内容；注意每一行都是字符串格式，在需要的地方加制表符和换行符调整格式；

