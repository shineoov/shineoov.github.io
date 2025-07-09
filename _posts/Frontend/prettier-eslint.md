---
layout: post
title: Prettier + ESLint
category: Frontend
tags:
  - Prettier
  - ESLint
date: 2025-03-02T02:08:00+09:00
last_modified_at: 2025-03-02T02:08:00+09:00
---
# Prettier


## Install

```bash
$ npm install --save-dev prettier

$ yarn add --dev prettier

```

## Settings

> `.prettierrc` 설정

```json
{  
  "semi": true,  
  "tabWidth": 2,  
  "printWidth": 80,  
  "singleQuote": true,  
  "trailingComma": "es5"  
}
```

> `.prettierignore` 설정

```text
node_modules  
build  
dist
```

## Run

```bash
$ npx prettier --write .

$ npx prettier --write src/index.js

$ npx prettier --write "src/**/*.{js,jsx,ts,tsx}"

```


>  `package.json` 설정  

```json
{
  "scripts": {
    "format": "prettier --write \"src/**/*.{js,jsx,ts,tsx,json,css,md}\""
  }
}

```

```bash
$ npm run format
```

# ESLint


## Install

```bash


# 1. ESLint 설치하기 
$ npm install --save-dev eslint 
$ yarn add --dev eslint

# 2. ESLint 설정 파일 생성하기 (대화형 설정) 
$ npx eslint --init


$ npx eslint .

```