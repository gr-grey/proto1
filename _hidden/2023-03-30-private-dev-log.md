---
layout: single
title: "Private Dev Log"
date: 2023-03-30
permalink: /hidden/2023/03/private-dev-log/
tags:
  - private
---

A dev log for my own. Contain trivial contents that are not worth sharing to the public.

## 2023-03-31

When trying to get vimrc code block to look pretty, I coverted vimrc to HTML format (:TOhtml, then :saveas),
copy paste the code block into the file, and the local page just turned into Gruvbox theme, very beautiful

<html>
<head>
<style type="text/css">
<!--
* { font-size: 1em; }
.Normal { color: #ffd7af; background-color: #262626; padding-bottom: 1px; }
.GruvboxYellow { color: #ffaf00; }
.String { color: #afaf00; }
.GruvboxAqua { color: #87af87; }
.LineNr { color: #767676; }
.GruvboxRed { color: #d75f5f; }
.GruvboxOrange { color: #ff8700; }
.Comment { color: #8a8a8a; }
-->
</style>

</head>

<body>
<pre id='vimCodeElement'>
<span id="L1" class="LineNr"> 1 </span><span class="comment">&quot; 2023/03/20 had problem with linebreak character after windows upgrade</span>
<span id="l58" class="linenr">58 </span><span class="gruvboxred">call</span> plug#<span class="normal">end</span><span class="gruvboxorange">()</span>
<span id="l59" class="linenr">59 </span>
<span id="l60" class="linenr">60 </span><span class="comment">&quot; swap gj/gk and j/k</span>
<span id="l61" class="linenr">61 </span><span class="gruvboxred">nnoremap</span> j gj
<span id="l62" class="linenr">62 </span><span class="gruvboxred">nnoremap</span> gj j
<span id="l63" class="linenr">63 </span><span class="gruvboxred">nnoremap</span> k gk
<span id="l64" class="linenr">64 </span><span class="gruvboxred">nnoremap</span> gk k
<span id="l65" class="linenr">65 </span>
</pre>
</body>
</html>

```html
<!doctype html public "-//w3c//dtd html 4.01//en" "http://www.w3.org/tr/html4/strict.dtd">
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8">
<title>~/.vimrc.html</title>
<meta name="generator" content="Vim/8.0">
<meta name="plugin-version" content="vim7.4_v2">
<meta name="syntax" content="vim">
<meta name="settings" content="number_lines,use_css,pre_wrap,no_foldcolumn,expand_tabs,line_ids,prevent_copy=">
<meta name="colorscheme" content="gruvbox">
<style type="text/css">
<!--
pre { white-space: pre-wrap; font-family: monospace; color: #ffd7af; background-color: #262626; }
body { font-family: monospace; color: #ffd7af; background-color: #262626; }
* { font-size: 1em; }
.Normal { color: #ffd7af; background-color: #262626; padding-bottom: 1px; }
.GruvboxYellow { color: #ffaf00; }
.String { color: #afaf00; }
.GruvboxAqua { color: #87af87; }
.LineNr { color: #767676; }
.GruvboxRed { color: #d75f5f; }
.GruvboxOrange { color: #ff8700; }
.Comment { color: #8a8a8a; }
-->
</style>

<script type='text/javascript'>
<!--

/* function to open any folds containing a jumped-to line before jumping to it */
function JumpToLine()
{
  var lineNum;
  lineNum = window.location.hash;
  lineNum = lineNum.substr(1); /* strip off '#' */

  if (lineNum.indexOf('L') == -1) {
    lineNum = 'L'+lineNum;
  }
  lineElem = document.getElementById(lineNum);
  /* Always jump to new location even if the line was hidden inside a fold, or
   * we corrected the raw number to a line ID.
   */
  if (lineElem) {
    lineElem.scrollIntoView(true);
  }
  return true;
}
if ('onhashchange' in window) {
  window.onhashchange = JumpToLine;
}

-->
</script>
</head>
<body onload='JumpToLine();'>
<pre id='vimCodeElement'>
<span id="L1" class="LineNr"> 1 </span><span class="Comment">&quot; 2023/03/20 had problem with linebreak character after windows upgrade</span>
<span id="L2" class="LineNr"> 2 </span><span class="Comment">&quot; add fileformat line to prevent weird things from happening.</span>
<span id="L3" class="LineNr"> 3 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">fileformat</span>=unix
<span id="L4" class="LineNr"> 4 </span><span class="Comment">&quot; absolute number for current line and relavetive numbers for everywhere else</span>
<span id="L5" class="LineNr"> 5 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">nu</span> <span class="GruvboxAqua">rnu</span>
<span id="L6" class="LineNr"> 6 </span>
<span id="L7" class="LineNr"> 7 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">updatetime</span>=250
<span id="L8" class="LineNr"> 8 </span><span class="Comment">&quot; map jj to backspace in insert mode</span>
<span id="L9" class="LineNr"> 9 </span><span class="GruvboxRed">imap</span> jj <span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">BS</span><span class="GruvboxOrange">&gt;</span>
<span id="L10" class="LineNr">10 </span><span class="GruvboxRed">imap</span> cw <span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">Esc</span><span class="GruvboxOrange">&gt;</span>ciw
<span id="L11" class="LineNr">11 </span>
<span id="L12" class="LineNr">12 </span><span class="Comment">&quot; remap jj to esc for input, visual and normal mode</span>
<span id="L13" class="LineNr">13 </span><span class="GruvboxRed">imap</span> jk <span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">Esc</span><span class="GruvboxOrange">&gt;</span>
<span id="L14" class="LineNr">14 </span><span class="GruvboxRed">imap</span> kj <span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">Esc</span><span class="GruvboxOrange">&gt;</span>
<span id="L15" class="LineNr">15 </span>
<span id="L16" class="LineNr">16 </span><span class="Comment">&quot; oo for inserting a new line and </span>
<span id="L17" class="LineNr">17 </span><span class="Comment">&quot; nmap oo o&lt;Esc&gt;k</span>
<span id="L18" class="LineNr">18 </span><span class="Comment">&quot; \o to insert a new line in normal mode</span>
<span id="L19" class="LineNr">19 </span><span class="GruvboxRed">nnoremap</span> <span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">Leader</span><span class="GruvboxOrange">&gt;</span>o o<span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">Esc</span><span class="GruvboxOrange">&gt;</span>0&quot;_D
<span id="L20" class="LineNr">20 </span>
<span id="L21" class="LineNr">21 </span><span class="Comment">&quot; theme: monokai</span>
<span id="L22" class="LineNr">22 </span><span class="GruvboxRed">syntax</span> <span class="GruvboxYellow">enable</span>
<span id="L23" class="LineNr">23 </span><span class="GruvboxRed">syntax</span> <span class="GruvboxYellow">on</span>
<span id="L24" class="LineNr">24 </span><span class="GruvboxRed">autocmd</span> <span class="GruvboxYellow">vimenter</span> * nested <span class="GruvboxRed">colorscheme</span> gruvbox
<span id="L25" class="LineNr">25 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">bg</span>=dark
<span id="L26" class="LineNr">26 </span>
<span id="L27" class="LineNr">27 </span><span class="Comment">&quot; cursors</span>
<span id="L28" class="LineNr">28 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">cursorline</span>
<span id="L29" class="LineNr">29 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">cursorcolumn</span>
<span id="L30" class="LineNr">30 </span><span class="Comment">&quot; steady block for visual mode, bar for insert</span>
<span id="L31" class="LineNr">31 </span><span class="GruvboxRed">let</span> &amp;t_SI <span class="Normal">=</span> <span class="String">&quot;\e[6 q&quot;</span>
<span id="L32" class="LineNr">32 </span><span class="GruvboxRed">let</span> &amp;t_EI <span class="Normal">=</span> <span class="String">&quot;\e[2 q&quot;</span>
<span id="L33" class="LineNr">33 </span>
<span id="L34" class="LineNr">34 </span><span class="Comment">&quot; don't let cursor go below the last 5 lines</span>
<span id="L35" class="LineNr">35 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">scrolloff</span>=5
<span id="L36" class="LineNr">36 </span>
<span id="L37" class="LineNr">37 </span><span class="Comment">&quot; fix tab behaviors</span>
<span id="L38" class="LineNr">38 </span><span class="GruvboxRed">filetype</span> <span class="GruvboxYellow">plugin</span> <span class="GruvboxYellow">indent</span> <span class="GruvboxYellow">on</span>
<span id="L39" class="LineNr">39 </span><span class="Comment">&quot; show existing tab with 4 spaces width</span>
<span id="L40" class="LineNr">40 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">tabstop</span>=4
<span id="L41" class="LineNr">41 </span><span class="Comment">&quot; when indenting with '&gt;', use 4 spaces width</span>
<span id="L42" class="LineNr">42 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">shiftwidth</span>=4
<span id="L43" class="LineNr">43 </span><span class="Comment">&quot; On pressing tab, insert 4 spaces</span>
<span id="L44" class="LineNr">44 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">expandtab</span>
<span id="L45" class="LineNr">45 </span>
<span id="L46" class="LineNr">46 </span><span class="Comment">&quot; incrementally highlight as searchring</span>
<span id="L47" class="LineNr">47 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">incsearch</span>
<span id="L48" class="LineNr">48 </span><span class="Comment">&quot; case smart insensitive search, still check caps if explicity search caps</span>
<span id="L49" class="LineNr">49 </span><span class="GruvboxRed">set</span> <span class="GruvboxAqua">ignorecase</span> <span class="GruvboxAqua">smartcase</span>
<span id="L50" class="LineNr">50 </span>
<span id="L51" class="LineNr">51 </span><span class="Comment">&quot; custom key bindings</span>
<span id="L52" class="LineNr">52 </span><span class="Comment">&quot; Ctrl+y to yank to system clipboard under visual mode</span>
<span id="L53" class="LineNr">53 </span><span class="GruvboxRed">vnoremap</span> <span class="GruvboxOrange">&lt;</span><span class="GruvboxOrange">C-y</span><span class="GruvboxOrange">&gt;</span> &quot;+y
<span id="L54" class="LineNr">54 </span>
<span id="L55" class="LineNr">55 </span><span class="Comment">&quot;intall plug-ins with vim-plug</span>
<span id="L56" class="LineNr">56 </span><span class="GruvboxRed">call</span> plug#<span class="Normal">begin</span><span class="GruvboxOrange">()</span>
<span id="L57" class="LineNr">57 </span>Plug <span class="String">'morhetz/gruvbox'</span>
<span id="L58" class="LineNr">58 </span><span class="GruvboxRed">call</span> plug#<span class="Normal">end</span><span class="GruvboxOrange">()</span>
<span id="L59" class="LineNr">59 </span>
<span id="L60" class="LineNr">60 </span><span class="Comment">&quot; swap gj/gk and j/k</span>
<span id="L61" class="LineNr">61 </span><span class="GruvboxRed">nnoremap</span> j gj
<span id="L62" class="LineNr">62 </span><span class="GruvboxRed">nnoremap</span> gj j
<span id="L63" class="LineNr">63 </span><span class="GruvboxRed">nnoremap</span> k gk
<span id="L64" class="LineNr">64 </span><span class="GruvboxRed">nnoremap</span> gk k
<span id="L65" class="LineNr">65 </span>
</pre>
</body>
</html>
<!-- vim: set foldmethod=manual : -->

```

## 2023-03-30 Add hidden column

No direct button to this category.
Mostly for keeping records of stuff not interesting enough for the public to see,
or posts that are unfinished and should be posted in the future.

- permalink /hidden
- tag private
- folder _hidden
- add hidden to config.yml collections
- add first post (this post).