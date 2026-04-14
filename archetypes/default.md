---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
description: "Desc Text."
date: '{{ .Date }}'
draft: true
author: "Hayden Plumley"
tags: ["Linux"]
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
---
