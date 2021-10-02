---
title: HTMLIFrameElement.contentDocument
slug: Web/API/HTMLIFrameElement/contentDocument
browser-compat: api.HTMLIFrameElement.contentDocument
translation_of: 'HTMLIFrameElement.contentDocument'
---
<p>Si l'<i lang="en">iframe</i> et le document parent de l'<i lang="en">iframe</i> sont de la <a href="/fr/docs/Web/Security/Same-origin_policy">même origine</a>, <code>HTMLIFrameElement.contentDocument</code> retourne un <a href="/fr/docs/Web/API/Document"><code>Document</code></a> (c'est à dire le document actif dans le contexte de navigation imbriqué du cadre). Sinon, il retourne <code>null</code>.</p>

<h2 id="examples">Exemples</code></h2>

<pre class="brush: js">var iframeDocument = document.getElementsByTagName("iframe")[0].contentDocument;

iframeDocument.body.style.backgroundColor = "blue";
// Cela passe la couleur d'arrière-plan de l'iframe en bleu.</pre>

<h2 id="specifications">Spécifications</h2>

<p>{{Specifications}}</p>

<h2 id="browser_compatibility">Compatibilité des navigateurs</h2>

<p>{{Compat}}</p>