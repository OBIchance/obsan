+++
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
date = {{ .Date }}
draft = true
author = "Obsan Muzemil"
updated = {{ now.Format "2006-01-02" }}
+++