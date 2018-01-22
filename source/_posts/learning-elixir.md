title: Elixir 学习记录
date: 2016-06-01 02:06:33
tags: elixir
categories: 学习记录
---

<blockquote class="blockquote-center">Elixir, 我中了你的毒</blockquote>

# Process

## 1. `[error] Too many processes` 默认最大线程数

默认是 262143，可以配置

```
elixir --erl "+P 5000000" file/to/run.exs
ELIXIR_ERL_OPTS="+P 5000000" elixir file/to/run.exs
```

Refs:
- [Maximum number of spawns](https://groups.google.com/forum/#!topic/elixir-lang-talk/No1Qq0huj_E)


## 2.
