---
title: window.open
slug: Web/API/Window/open
tags:
  - DOM
  - DOM_0
translation_of: Web/API/Window/open
---
<p>{{ ApiRef() }}</p>

<h3 id="D.C3.A9finition">Définition</h3>

<p>Crée une nouvelle fenêtre de navigation secondaire et y charge la ressource référencée.</p>

<h3 id="Syntaxe">Syntaxe</h3>

<pre class="brush: js">var windowObjectReference = window.open(strUrl, strWindowName[, strWindowFeatures]);</pre>

<h3 id="Valeur_renvoy.C3.A9e_et_param.C3.A8tres">Valeur renvoyée et paramètres</h3>

<dl>
 <dt><code>WindowObjectReference</code></dt>
 <dd>Il s'agit de la référence pointant vers la fenêtre de navigation nouvellement créée. Cette référence est la valeur renvoyée par la méthode <code>open()</code> ; elle sera à <code>null</code> si pour une raison ou une autre l'appel n'a pas réussi à ouvrir une fenêtre. Une variable globale est le meilleur endroit pour stocker une telle référence. Il est alors possible, par exemple, de l'utiliser pour obtenir certaines propriétés de la nouvelle fenêtre, ou accéder à certaines de ses méthodes, à condition que la relation entre votre fenêtre principale et votre fenêtre secondaire se conforme avec les paramètres de sécurité de même origine (<em><a href="http://www.mozilla.org/projects/security/components/same-origin.html">Same origin policy</a> security requirements</em> ).</dd>
 <dt><code>strUrl</code></dt>
 <dd>Il s'agit de l'URL à charger dans la fenêtre nouvellement créée. <var>strUrl</var> peut être un document HTML, un fichier image, ou tout autre type de fichier géré par le navigateur.</dd>
 <dt><code>strWindowName</code></dt>
 <dd>Il s'agit de la chaîne d'identification de la nouvelle fenêtre. Celle-ci peut être utilisée pour être la cible de liens et de formulaires lorsque l'attribut <code>target</code> d'un élément <code>&lt;a&gt;</code> ou d'un élément <code>&lt;form&gt;</code> est spécifié. Cette chaîne ne peut contenir d'espaces. <var>strWindowName</var> ne spécifie pas le titre de la fenêtre, juste son nom interne.</dd>
 <dt><code>strWindowFeatures</code></dt>
 <dd>Paramètre optionnel. Il s'agit de la chaîne listant les fonctionnalités de la nouvelle fenêtre de navigation (et de ses barres d'outils). Cette chaîne ne peut contenir d'espaces. Chaque fonctionnalité doit être séparée par une virgule au sein de la chaîne de caractères.</dd>
</dl>

<h3 id="Description">Description</h3>

<p>La méthode <code>open()</code> crée une nouvelle fenêtre secondaire de navigation, de façon similaire au choix de l'option Nouvelle fenêtre du menu Fichier. Le paramètre <var>strUrl</var> spécifie l'URL à récupérer et à charger dans la nouvelle fenêtre. Si <var>strUrl</var> est une chaîne vide, une nouvelle fenêtre vide de tout contenu (l'URL <code>about:blank</code> sera chargée) est créée avec les barres d'outils par défaut de la fenêtre principale.</p>

<p>Notez que les URL distantes ne seront pas chargées instantanément. Lorsque l'appel à <code>window.open()</code> se termine et renvoie sa valeur, la fenêtre contient toujours <code>about:blank</code>. Le chargement proprement dit de l'URL est reporté et ne commence effectivement qu'après la fin de l'exécution du bloc de script courant. La création de la fenêtre d'une part et le chargement de la ressource référencée d'autre part sont faits de manière asynchrone.</p>

<h4 id="Exemples">Exemples</h4>

<pre class="brush:js">var windowObjectReference;
var strWindowFeatures = "menubar=yes,location=yes,resizable=yes,scrollbars=yes,status=yes";

function openRequestedPopup() {
  windowObjectReference = window.open("http://www.cnn.com/", "CNN_WindowName", strWindowFeatures);
}</pre>

<pre class="brush:js">var windowObjectReference;

function openRequestedPopup() {
  windowObjectReference = window.open(
    "http://www.domainname.ext/path/ImageFile.png",
    "DescriptiveWindowName",
    "resizable,scrollbars,status"
  );
}</pre>



<p>Si une fenêtre du nom <var>strWindowName</var> existe déjà, alors, au lieu d'ouvrir une nouvelle fenêtre, <var>strUrl</var> est chargée dans cette fenêtre existante. Dans ce cas, la valeur renvoyée par la méthode est la fenêtre existante, et <var>strWindowFeatures</var> est ignorée. Fournir une chaîne vide pour <var>strUrl</var> est une manière d'obtenir une référence à une fenêtre ouverte par son nom sans en changer l'adresse. Si vous désirez ouvrir une nouvelle fenêtre à chaque appel de <code>window.open()</code>, il faut utiliser la nom réservé <var>_blank</var> pour <var>strWindowName</var>.</p>

<div class="note">
<p><strong>Note :</strong> À propos de l'utilisation de <code>window.open</code> pour ré-ouvrir ou donner le focus à une fenêtre existante portant un nom connu du domaine : Cette fonctionalité n'est pas valide pour tous les navigateurs et qui plus est avec des comportement variables. Firefox (50.0.1) fonctionne comme il est décrit ici : depuis le même domaine+port la ré-exécution de window.open avec le même nom va accéder à la fenêtre précédemment ouverte.  Google Chrome (55.0.2883.87 m ) pour sa part ne l'exécutera qu'à partir du même parent (nommé "opener", comme si les fenêtres étaient créées avec une dépendance et uniquement avec <code>window.open</code>). Il s'agit là d'une limitation considérable ce qui génère une incroyable complexité de développement parfois sans issue. Firefox récupère le handle vers toutes les fenêtres du domaine dont le nom est connu, ce qui permet d'accéder à leur données, mais il ne peut exécuter une commande <code>HTMLElement.focus()</code> vers l'une quelconque de ces fenêtres, ce qui interdit de réouvrir la fenêtre pourtant connue comme active. Pour sa part Chrome peut redonner le focus à une fenêtre (onglet) enfant mais l'opération est impossible entre frères et depuis l'enfant vers le parent. Quant aux autres fenêtres du même domaine (même famille ?) non ouvertes avec <code>window.open</code> elles sont inconnues et<code> window.open('',&lt;name&gt;,'')</code> ouvrira alors des doublons. La fonction <code>hw=window.open('',strWindowName<strong> </strong> ,'')</code> est pourtant la seule qui permette de récupérer un handle en connaissant un nom et théoriquement éviter la création de doublons, mais pour l'instant, 22/02/2017, les différences de comportement des navigateurs ne permettent de l'utiliser que de manière partielle dans des cas très restreints.</p>
</div>

<p><var>strWindowFeatures</var> est une chaîne optionnelle contenant une liste, séparée par des virgules, de fonctionnalités demandées pour la nouvelle fenêtre. Après qu'une fenêtre soit ouverte, vous ne pouvez pas utiliser JavaScript pour changer ses fonctionnalités et ses barres d'outils. Si <var>strWindowName</var> ne spécifie pas une fenêtre existante et si vous ne fournissez pas le paramètre <var>strWindowFeatures</var> (ou si celui-ci est une chaîne vide), la nouvelle fenêtre secondaire comportera les barres d'outils par défaut de la fenêtre principale.</p>

<p>Si le paramètre <var>strWindowFeatures</var> est utilisé et si aucune information de taille n'est définie, les dimensions de la nouvelle fenêtre seront les mêmes que celles de la fenêtre ouverte la plus récemment.</p>

<p>Si le paramètre <var>strWindowFeatures</var> est utilisé et qu'aucune information de position n'est définie, les coordonnées du coin en haut à gauche de la nouvelle fenêtre seront décalées de 22 pixels vers le bas et vers la droite par rapport à celles de la fenêtre ouverte le plus récemment. Un décalage est utilisé par tous les concepteurs de navigateurs (il est de 29 pixels dans Internet Explorer 6 SP2 avec le thème par défaut) et son but est d'aider l'utilisateur à remarquer l'ouverture d'une nouvelle fenêtre. Si la fenêtre ouverte le plus récemment était maximisée, il n'y aura pas de décalage et la nouvelle fenêtre secondaire sera maximisée également.</p>

<p><strong>Si le paramètre <var>strWindowFeatures</var> est défini, les fonctionnalités qui ne sont pas listées, explicitement demandées dans la chaîne, seront désactivées ou enlevées</strong> (à l'exception de <var>titlebar</var> et <var>close</var> qui sont par défaut à <var>yes</var>).</p>

<div class="note">
<p><strong>Note :</strong> Si vous utilisez le paramètre <var>strWindowFeatures</var>, listez uniquement les fonctionnalités dont vous avez besoin, qui doivent être activées ou affichées. Les autres (à l'exception de <var>titlebar</var> et <var>close</var>) seront désactivées ou enlevées.</p>
</div>

<h4 id="Fonctionnalit.C3.A9s_de_position_et_de_taille">Fonctionnalités de position et de taille</h4>

<p><a href="#Note_sur_les_corrections_d.27erreurs_de_position_et_de_dimension">Note sur les corrections d'erreurs de position et de dimension</a></p>

<p>{{bug(176320) }}</p>

<p><a href="#Note_sur_les_priorit.C3.A9s">Note sur les priorités</a></p>

<dl>
 <dt>left</dt>
 <dd>Spécifie la distance à laquelle la nouvelle fenêtre est placée depuis le bord gauche de la zone de travail destinée aux applications du système d'exploitation de l'utilisateur jusqu'à la bordure extérieure (bordure de redimensionnement) de la fenêtre de navigation. La nouvelle fenêtre ne peut pas être positionnée initialement hors de l'écran.</dd>
 <dt>top</dt>
 <dd>Spécifie la distance à laquelle la nouvelle fenêtre est placée depuis le bord supérieur de la zone de travail destinée aux applications du système d'exploitation de l'utilisateur jusqu'à la bordure extérieure (bordure de redimensionnement) de la fenêtre de navigation. La nouvelle fenêtre ne peut pas être positionnée initialement hors de l'écran.</dd>
 <dt>height</dt>
 <dd>Spécifie la hauteur de la zone de contenu, l'espace de visualisation de la nouvelle fenêtre secondaire, en pixels. La valeur de hauteur comprend celle de la barre de défilement horizontale si celle-ci est présente. La valeur minimale requise est 100.</dd>
 <dt>width</dt>
 <dd>Spécifie la largeur de la zone de contenu, l'espace de visualisation de la nouvelle fenêtre secondaire, en pixels. La valeur de largeur comprend celle de la barre de défilement verticale si celle-ci est présente. Elle ne prend pas en compte un éventuel panneau latéral si celui-ci est déplié. La valeur minimale requise est 100.</dd>
 <dt>screenX</dt>
 <dd>Mozilla.</dd>
 <dt>screenY</dt>
 <dd>Mozilla.</dd>
 <dt>outerHeight</dt>
 <dd><p>Spécifie la hauteur de toute la fenêtre de navigation en pixels. Cette valeur outerHeight comprend toute barre d'outils présente, la barre de défilement horizontale de la fenêtre (si présente) et les bordures inférieures et supérieures. La valeur minimale requise est 100.</p>
 <div class="note"><p><strong>Note :</strong> étant donné que la barre de titre est toujours visible, demander une valeur outerHeight=100 rendra la valeur innerHeight de la fenêtre de navigation plus petite que les 100 pixels minimaux habituels.</p></div></dd>
 <dt>outerWidth</dt>
 <dd>Spécifie la largeur de toute la fenêtre de navigation en pixels. Cette valeur outerWidth comprend la barre de défilement verticale (si présente) et les bords gauche et droits de la fenêtre.</dd>
 <dt>innerHeight</dt>
 <dd>de la zone de contenu, l'espace de visualisation de la nouvelle fenêtre secondaire, en pixels. La valeur <code>innerHeight</code> comprend la hauteur de la barre de défilement horizontale si celle-ci est présente. La valeur minimale requise est 100.</dd>
 <dt>innerWidth</dt>
 <dd>de la zone de contenu, l'espace de visualisation de la nouvelle fenêtre secondaire, en pixels. La valeur <code>innerWidth</code> comprend la largeur de la barre de défilement verticale si celle-ci est présente. Elle ne prend pas en compte un éventuel panneau latéral si celui-ci est déplié. La valeur minimale requise est 100.</dd>
</dl>

<h4 id="Fonctionnalit.C3.A9s_de_barres_d.27outils_et_de_chrome">Fonctionnalités de barres d'outils et de chrome</h4>

<dl>
 <dt>menubar</dt>
 <dd><p>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire disposera d'une barre de menus. Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à toujours avoir une barre de menus en positionnant <code>dom.disable_window_open_feature.menubar</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p></dd>
 <dt>toolbar</dt>
 <dd><p>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire disposera d'une barre de navigation (boutons Précédente, Suivante, Actualiser et Arrêter). En plus de la barre de navigation, les navigateurs basés sur Mozilla afficheront également la barre d'onglets si elle est toujours visible et présente dans la fenêtre parente. Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à toujours afficher la barre de navigation en positionnant <code>dom.disable_window_open_feature.toolbar</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p></dd>
 <dt>location</dt>
 <dd><p>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire affichera une barre d'adresse (ou d'emplacement).Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à toujours afficher la barre d'adresse en positionnant <code>dom.disable_window_open_feature.location</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>. Notez qu'Internet Explorer 7 force la présence de la barre d'adresse : « Nous pensons que la barre d'adresse est aussi importante pour les utilisateurs <strong>à voir dans les fenêtres pop-up</strong>. Une barre d'adresse manquante crée une opportunité pour un fraudeur de contrefaire une adresse. Pour aider à contrer cela, <strong>IE7 va afficher une barre d'adresse sur toutes les fenêtres de navigation pour aider les utilisateurs à voir où ils sont</strong>. » provenant de <a href="http://blogs.msdn.com/ie/archive/2005/11/21.aspx">Better Website Identification</a>. Il est également dans les intentions de Mozilla de forcer la présence de la Barre d'adresse dans une prochaine version de Firefox : {{bug(337344) }}</p></dd>
 <dt>directories</dt>
 <dd><p>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire affichera la barre personnelle dans les navigateurs Netscape 6.x, Netscape 7.x, Mozilla et Firefox. Dans Internet Explorer 5+, la barre de liens sera affichée. En plus de la barre personnelle, les navigateurs Mozilla afficheront la barre de navigation du site si celle-ci est visible et présente dans la fenêtre parente. Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à toujours afficher leur barre personnelle en positionnant<code>dom.disable_window_open_feature.directories</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p></dd>
 <dt>personalbar</dt>
 <dd>Similaire à <var>directories</var>, mais ne fonctionne que dans Netscape et les navigateurs basés sur Mozilla.</dd>
 <dt>status</dt>
 <dd>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire disposera d'une barre d'état. Les utilisateurs peuvent forcer la présence de la barre d'état dans tous les navigateurs basés sur Mozilla, dans Internet Explorer 6 SP2 (<a href="#Note_sur_les_probl.C3.A8mes_de_s.C3.A9curit.C3.A9_li.C3.A9s_.C3.A0_la_pr.C3.A9sence_de_la_barre_d.27.C3.A9tat">Note sur la barre d'état avec XP SP2</a>) et dans Opera 6+. Les réglages par défaut des navigateurs récents basés sur Mozilla et Firefox 1.0 forcent la présence de la barre d'état.</dd>
</dl>

<h4 id="Fonctionnalit.C3.A9s_relatives_.C3.A0_la_fen.C3.AAtre">Fonctionnalités relatives à la fenêtre</h4>

<dl>
 <dt>resizable</dt>
 <dd><p>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire sera redimensionnable À partir de la version 1.4, les navigateurs basés sur Mozilla ont une zone de redimensionnement à l'extrémité droite de la barre d'état. Cela permet de s'assurer que les utilisateurs peuvent toujours redimensionner la fenêtre même si l'auteur de la page Web a demandé que cette fenêtre secondaire ne soit pas redimensionnable. Dans ce cas, les icônes de maximisation/restauration dans la barre de titre de la fenêtre seront désactivées et les bordures de la fenêtre ne permettront pas de la redimensionner, mais celle-ci pourra toujours être redimensionnée via cette zone de la barre d'état. Il est probable qu'à partir de Firefox 3, toutes les fenêtres secondaires seront redimensionnables ({{ Bug(177838) }})</p>
<div class="note">
<p><strong>Note :</strong> pour des raisons d'accessibilité, il est vivement recommandé de toujours définir cette fonctionnalité à <var>yes</var>.</p>
</div>
 <p>Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à être facilement redimensionnables en positionnant <code>dom.disable_window_open_feature.resizable</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p>
</dd>
 <dt>scrollbars</dt>
 <dd><p>Si cette fonctionnalité est définie à <var>yes</var>, la nouvelle fenêtre secondaire affichera des barres de défilement horizontales et/ou verticales si le document ne rentre pas dans la zone d'affichage de la fenêtre.</p>
<div class="note">
<p><strong>Note :</strong> pour des raisons d'accessibilité, il est vivement recommandé de toujours définir cette fonctionnalité à <var>yes</var>.</p>
</div>
<p>Les utilisateurs de Mozilla et Firefox peuvent obliger cette option à être toujours activée en positionnant <code>dom.disable_window_open_feature.scrollbars</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p></dd>
 <dt>dependent</dt>
 <dd>Si définie à <var>yes</var>, la nouvelle fenêtre est dite dépendante de sa fenêtre parente. Une fenêtre dépendante se ferme lorsque sa fenêtre parente est fermée. Une fenêtre dépendante est réduite dans la barre des tâches uniquement lorsque sa fenêtre parente est réduite. Sur les plateformes Windows, une fenêtre dépendante n'est pas affichée sur la barre des tâches. Elle reste également en avant-plan de la fenêtre parente. Les fenêtres dépendantes n'existent pas sous Mac OS X, cette option y sera ignorée. La suppression complète de cette fonctionnalité sur toutes les plateformes est en cours de discussion ({{ Bug(214867) }}) Dans Internet Explorer 6, le plus proche équivalent à cette fonctionnalité est la méthode <code>showModelessDialog()</code>.</dd>
 <dt>modal</dt>
 <dd><p><strong>Note</strong> : à partir de Mozilla 1.2.1, cette fonctionnalité requiert le privilège <code>UniversalBrowserWrite</code> ({{ Bug(180048) }}). Sans ce privilège, elle est équivalente à <code>dependent</code>. Si définie à <var>yes</var>, la nouvelle fenêtre est dite modale. L'utilisateur ne peut pas retourner à la fenêtre principale tant que la fenêtre modale n'est pas fermée. Une fenêtre modale typique est créée par la <a href="fr/DOM/window.alert">fonction alert()</a>. Le comportement exact des fenêtres modales dépend de la plateforme et de la version de Mozilla. L'équivalent de cette fonctionnalité dans Internet Explorer 6 est la méthode <code>showModalDialog()</code>.</p></dd>
 <dt>dialog</dt>
 <dd><p>La fonctionnalité <code>dialog</code> retire toutes les icônes (restaurer, réduire, agrandir) de la barre de titre de la fenêtre, laissant uniquement le bouton de fermeture. Mozilla 1.2+ et Netscape 7.1 afficheront les autres commandes du menu système (dans Firefox 1.0 et Netscape 7.0x, le menu de commandes système n'est pas associé avec l'icône de Firefox/Netscape 7.0x à l'extrémité gauche de la barre de titre, il s'agit probablement d'un bug. Il est possible d'accéder au menu de commandes système avec un clic droit sur la barre de titre). Les fenêtres de dialogue sont des fenêtres qui n'ont pas d'icône de commande système de réduction ni d'agrandissement/restauration dans la barre de titre, ni dans les choix correspondants du menu de commandes système. On les appelle dialogues car leur utilisation normale est uniquement de notifier une information et d'être ensuite immédiatement fermées. Sur les systèmes Mac, les fenêtres de dialogue ont une bordure différente et peuvent prendre la forme de<em>sheets</em>.</p></dd>
 <dt>minimizable</dt>
 <dd><p>Ce paramètre peut uniquement s'appliquer à des fenêtres de dialogue ; l'utilisation de « minimizable » nécessite <code>dialog=yes</code>. Si <code>minimizable</code> est défini à <var>yes</var>, le nouveau dialogue aura une commande système de réduction dans la barre de titre et il sera possible de le réduire dans la barre des tâches. Toute fenêtre n'étant pas un dialogue est toujours réductible, cette option y sera donc ignorée.</p></dd>
 <dt>fullscreen</dt>
 <dd><p>Ce paramètre ne fonctionne plus dans Internet Explorer 6 SP2 de la façon dont il le faisait dans Internet Explorer 5.x. La barre des tâches de Windows, ainsi que la barre de titre et la barre d'état de la fenêtre ne sont ni visibles, ni accessibles lorsque le mode plein écran est activé dans IE 5.x. <code>fullscreen</code> ennuie toujours les utilisateurs disposant d'un grand écran ou de plusieurs écrans. Obliger les autres utilisateurs à passer en plein écran avec <code>fullscreen</code> est également extrêmement impopulaire et est considéré comme une tentative impolie d'imposer les préférences d'affichage de l'auteur à l'utilisateur. <code>fullscreen</code> ne fonctionne plus vraiment dans Internet Explorer 6 SP2.</p></dd>
</dl>

<h4 id="Fonctionnalit.C3.A9s_n.C3.A9cessitant_des_privil.C3.A8ges">Fonctionnalités nécessitant des privilèges</h4>

<p>Les fonctionnalités suivantes nécessitent le privilège <code>UniversalBrowserWrite</code>, autrement elles seront ignorées. Les scripts chrome bénéficient de ce privilège automatiquement, les autres doivent le demander via le <a href="fr/PrivilegeManager">PrivilegeManager</a>.</p>

<dl>
 <dt>chrome</dt>
 <dd><p><strong>Note</strong> : à partir de Mozilla 1.7/Firefox 0.9, cette fonctionnalité requiert le privilège <code>UniversalBrowserWrite</code> ({{ Bug(244965) }}). Sans ce privilège, elle est ignorée. Si définie à <var>yes</var>, la page est chargée en tant qu'unique contenu de la fenêtre, sans aucun autre élément de l'interface de navigation. Il n'y aura pas de menu contextuel défini par défaut, et aucun des raccourcis clavier standard ne fonctionnera. La page est supposée fournir sa propre interface utilisateur, et cette fonctionnalité est habituellement utilisée pour ouvrir des documents XUL (les dialogues standard comme la console JavaScript sont ouverts de cette façon).</p></dd>
 <dt>titlebar</dt>
 <dd><p>Par défaut, toutes les nouvelles fenêtres secondaires ont une barre de titre. Si défini à <var>no</var>, ce paramètre enlève la barre de titre de la nouvelle fenêtre secondaire. Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à toujours avoir une barre de titre en positionnant<code>dom.disable_window_open_feature.titlebar</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p></dd>
 <dt>alwaysRaised</dt>
 <dd>Si défini à <var>yes</var>, la nouvelle fenêtre sera toujours affichée par dessus les autres fenêtres du navigateur, qu'elle soit active ou non.</dd>
 <dt>alwaysLowered</dt>
 <dd><p>Si défini à <var>yes</var>, la nouvelle fenêtre créé flottera par dessous sa propre fenêtre parente tant que celle-ci n'est pas réduite. Les fenêtres utilisant alwaysLowered sont souvent appelées pop-under. Celles-ci ne peuvent pas passer au dessus de leur fenêtre parente, mais celle-ci peut être réduite. Dans Netscape 6.x, les fenêtres alwaysLowered n'ont pas de commande système de réduction ni de commande système restaurer/agrandir.</p></dd>
 <dt>z-lock</dt>
 <dd>Identique à <code>alwaysLowered</code>.</dd>
 <dt>close</dt>
 <dd><p>Lorsque défini à <var>no</var>, ce paramètre supprime l'icône système de fermeture de la fenêtre, et l'élément de menu correspondant. Il fonctionnera uniquement pour les fenêtres de dialogue (avec la fonctionnalité <code>dialog</code> activée). <code>close=no</code> a précédence sur <code>minimizable=yes</code>.Les utilisateurs de Mozilla et Firefox peuvent obliger les nouvelles fenêtres à toujours avoir un bouton de fermeture en positionnant <code>dom.disable_window_open_feature.close</code> à <var>true</var> dans <kbd>about:config</kbd> ou dans leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>.</p></dd>
</dl>

<p>Les fonctionnalités de position et de taille nécessitent d'être accompagnées d'un nombre. Les fonctionnalités de barres d'outils et de fenêtre peuvent être accompagnées de <var>yes</var> (oui) ou <var>no</var> (non) ; il est également possible d'utiliser <var>1</var> à la place de <var>yes</var> et <var>0</var> à la place de <var>no</var>. Les fonctionnalités de barres d'outils et de fenêtre acceptent aussi une forme raccourcie : vous pouvez activer une fonctionnalité en listant simplement son nom dans la chaîne <var>strWindowFeatures</var>. Si vous fournissez le paramètre <var>strWindowFeatures</var>, les fonctionnalités <code>titlebar</code> et <code>close</code> sont toujours à <var>yes</var> par défaut, mais les autres fonctionnalités présentant un choix <var>yes</var>/<var>no</var> seront à <var>no</var> par défaut et seront donc désactivées.</p>

<p>Exemple :</p>

<pre class="brush: html">&lt;script type="text/javascript"&gt;
var WindowObjectReference; // variable globale

function openRequestedPopup() {
  WindowObjectReference = window.open("http://www.nomdedomaine.ext/chemin/FichierImg.png",
        "NomDeFenetreDescriptif",
        "width=420,height=230,resizable,scrollbars=yes,status=1");
}
&lt;/script&gt;
</pre>

<p>Dans cet exemple, la fenêtre sera redimensionnable, elle affichera des barres de défilement si nécessaire (si le contenu dépasse les dimensions de fenêtre demandées) et affichera la barre d'état. Elle n'affichera pas la barre de menus ni la barre d'adresse. Comme l'auteur connaissait la taille de l'image (400 pixels de large et 200 pixels de haut), il a ajouté les marges appliquées à l'élément racine dans Internet Explorer 6, c'est-à-dire 15 pixels en haut, 15 pixels en bas, 10 pixels à gauche et 10 pixels à droite.</p>

<h3 id="Bonnes_pratiques">Bonnes pratiques</h3>

<pre class="brush: html">&lt;script type="text/javascript"&gt;
var WindowObjectReference = null; // variable globale

function openFFPromotionPopup() {
  if (WindowObjectReference == null || WindowObjectReference.closed) {
    /* si le pointeur vers l'objet window n'existe pas, ou s'il existe
       mais que la fenêtre a été fermée */
    WindowObjectReference = window.open("http://www.spreadfirefox.com/",
           "PromoteFirefoxWindowName", "resizable=yes,scrollbars=yes,status=yes");
    /* alors, création du pointeur. La nouvelle fenêtre sera créée par dessus
       toute autre fenêtre existante. */
  }
  else {
    WindowObjectReference.focus();
    /* sinon, la référence à la fenêtre existe et la fenêtre n'a pas été
       fermée: la fenêtre est peut-être minimisée ou derrière la fenêtre
       principale. Par conséquent, on peut l'amener par dessus les autres à
       l'aide de la méthode focus(). Il n'est pas nécessaire de recréer la fenêtre
       ou de recharger la ressource référencée. */
  };
}
&lt;/script&gt;

(...)

&lt;p&gt;&lt;a href="http://www.spreadfirefox.com/" target="PromoteFirefoxWindowName" onclick="openFFPromotionPopup(); return false;" title="Ce lien créera une nouvelle fenêtre ou en réutilisera une déjà ouverte"&gt;Promouvoir
l'adoption de Firefox&lt;/a&gt;&lt;/p&gt;
</pre>

<p>Le code ci-dessus résout un certain nombre de problèmes d'utilisabilité (<em>usability</em> ) relatif aux liens ouvrant des fenêtres secondaires. Le but du <code>return false</code> dans le code est d'annuler l'action par défaut du lien : si le gestionnaire d'évènements onclick est exécuté, il n'est pas nécessaire d'exécuter l'action par défaut du lien. Mais si JavaScript est désactivé ou non existant dans le navigateur de l'utilisateur, c'est l'évènement onclick qui est ignoré et le navigateur charge la ressource référencée dans le cadre ou la fenêtre portant le nom « PromoteFirefoxWindowName ». Si aucun cadre ni fenêtre ne porte ce nom, le navigateur créera une nouvelle fenêtre et l'appellera « PromoteFirefoxWindowName ».</p>

<p>Plus d'informations sur l'utilisation de l'attribut target :</p>

<p><a href="http://www.la-grange.net/w3c/html4.01/present/frames.html#h-16.3.2">HTML 4.01, section 16.3.2 La sémantique de cible</a></p>

<p><a href="http://www.htmlhelp.com/fr/faq/html/links.html#new-window">Comment créer un lien qui ouvre une nouvelle fenêtre?</a></p>

<p>Vous pouvez également paramétriser la fonction pour la rendre polyvalente, fonctionnelle dans plus de situations, et donc réutilisable dans d'autres scripts et pages Web :</p>

<pre class="brush: html">&lt;script type="text/javascript"&gt;
var WindowObjectReference = null; // variable globale

function openRequestedPopup(strUrl, strWindowName) {
  if(WindowObjectReference == null || WindowObjectReference.closed) {
    WindowObjectReference = window.open(strUrl, strWindowName,
           "resizable=yes,scrollbars=yes,status=yes");
  }
  else {
    WindowObjectReference.focus();
  };
}
&lt;/script&gt;
(...)

&lt;p&gt;&lt;a href="http://www.spreadfirefox.com/" target="PromoteFirefoxWindow" onclick="openRequestedPopup(this.href, this.target); return false;" title="Ce lien créera une nouvelle fenêtre ou en réutilisera une déjà ouverte"&gt;Promouvoir l'adoption de Firefox&lt;/a&gt;&lt;/p&gt;
</pre>

<p>Vous pouvez également ne permettre à cette fonction que d'ouvrir une seule fenêtre secondaire et de la réutiliser pour d'autres liens de cette manière :</p>

<pre class="brush: html">&lt;script type="text/javascript"&gt;
var WindowObjectReference = null; // variable globale
var PreviousUrl; /* variable globale qui stockera l'URL actuellement
                    chargée dans la fenêtre secondaire */

function openRequestedSinglePopup(strUrl)
{
  if(WindowObjectReference == null || WindowObjectReference.closed)
  {
    WindowObjectReference = window.open(strUrl, "SingleSecondaryWindowName",
           "resizable=yes,scrollbars=yes,status=yes");
  }
  else if(previousUrl != strUrl)
  {
    WindowObjectReference = window.open(strUrl, "SingleSecondaryWindowName",
        "resizable=yes,scrollbars=yes,status=yes");
    /* si la ressource à charger est différente, on la charge dans la fenêtre
       secondaire déjà ouverte, et on ramène ensuite celle-ci par dessus
       sa fenêtre parente. */
    WindowObjectReference.focus();
  }
  else
  {
    WindowObjectReference.focus();
  };
  PreviousUrl = strUrl;
  /* explication : on stocke l'URL courante afin de pouvoir la comparer
     dans le cas d'un autre appel à cette fonction. */
}
&lt;/script&gt;

(...)

&lt;p&gt;&lt;a href="http://www.spreadfirefox.com/" target="SingleSecondaryWindowName"
onclick="openRequestedSinglePopup(this.href); return false;"
title="Ce lien créera une nouvelle fenêtre ou en réutilisera une déjà ouverte"&gt;Promouvoir
l'adoption de Firefox&lt;/a&gt;&lt;/p&gt;
&lt;p&gt;&lt;a href="http://www.mozilla.org/support/firefox/faq"
target="SingleSecondaryWindowName"
onclick="openRequestedSinglePopup(this.href); return false;"
title="Ce lien créera une nouvelle fenêtre ou en réutilisera une déjà ouverte"&gt;FAQ
de Firefox&lt;/a&gt;&lt;/p&gt;
</pre>

<h3 id="FAQ">FAQ</h3>

<dl>
 <dt>Comment empêcher l'apparition du message de confirmation demandant à l'utilisateur s'il veut fermer la fenêtre ?</dt>
 <dd>Vous ne pouvez pas. <strong>La règle est que les nouvelles fenêtres qui ne sont pas ouvertes par JavaScript ne peuvent pas être fermées par JavaScript.</strong> La console JavaScript dans les navigateurs basés sur Mozilla affichera le message d'avertissement suivant : <code>"Scripts may not close windows that were not opened by script."</code> Si c'était possible, l'historique des URL visitées durant la session de navigation serait perdu au détriment de l'utilisateur. <a href="fr/DOM/window.close">Plus de détails sur la méthode window.close()</a></dd>
 <dt>Comment ramener la fenêtre si elle est réduite ou masquée par une autre fenêtre ?</dt>
 <dd>Vérifiez d'abord l'existence de la référence à l'objet window, et si celle-ci existe et n'a pas été fermée, utilisez la méthode <a href="fr/DOM/window.focus">focus()</a>. Il n'y a pas d'autre manière fiable. Un <a href="#Bonnes_pratiques">exemple montrant comment utiliser la méthode focus()</a> est présenté ci-dessus.</dd>
 <dt>Comment forcer une fenêtre à être agrandie/maximisée ?</dt>
 <dd>Ce n'est pas possible. Tous les fabricants de navigateurs essaient de rendre l'ouverture d'une nouvelle fenêtre visible et remarquée par les utilisateurs, afin d'éviter de les désorienter.</dd>
 <dt>Comment désactiver le redimensionnement des fenêtres ou supprimer les barres d'outils ?</dt>
 <dd>Il n'est pas possible de l'imposer. Les utilisateurs de navigateurs basés sur Mozilla ont un contrôle absolu sur les fonctionnalités des fenêtres comme le fait de pouvoir les redimensionner, de les faire défiler et la présence de barres d'outils via les préférences utilisateur dans <code>about:config</code>. Comme vos utilisateurs sont ceux qui sont supposés utiliser de telles fenêtres (et non vous en tant qu'auteur), le mieux est d'éviter d'interférer avec leurs habitudes et préférences. Il est recommandé de toujours positionner la redimensionnabilité et la présence de barres de défilement (si nécessaire) à <code>yes</code> pour assurer l'accessibilité au contenu et l'utilisabilité des fenêtres. Ceci est dans l'intérêt tant de l'auteur Web que de ses utilisateurs.</dd>
 <dt>Comment redimensionner une fenêtre en fonction de son contenu ?</dt>
 <dd><p>Ce n'est pas faisable de manière fiable, car les utilisateurs peuvent empêcher la fenêtre d'être redimensionnée en décochant la case <code>Édition/Préférences/Avancé/Scripts &amp; plugins/Autoriser les scripts à/ Déplacer ou redimensionner des fenêtres existantes</code> dans Mozilla ou <code>Outils/Options/Contenu/Activer JavaScript/Bouton Avancé/Déplacer ou redimensionner des fenêtres existantes</code> dans Firefox, ou en positionnant <code>dom.disable_window_move_resize</code> à <var>true</var> dans <kbd>about:config</kbd> ou encore en éditant leur <a href="http://www.mozilla.org/support/firefox/edit#user">fichier user.js</a>. En général, les utilisateurs désactivent le déplacement et le redimensionnement des fenêtres, étant donné qu'autoriser les scripts à le faire a été la source d'énormément d'abus dans le passé, et les rares scripts qui n'en abusent pas sont souvent incorrects dans leur manière de redimensionner la fenêtre. 99% de ces scripts désactivent le redimensionnement manuel des fenêtres et les barres de défilement alors qu'ils devraient en fait activer ces deux fonctionnalités pour permettre un mécanisme de récupération sain et circonspect dans le cas où leurs calculs s'avèreraient incorrects.</p><p>La méthode <a href="fr/DOM/window.sizeToContent">sizeToContent()</a> de l'objet window est également désactivée si l'utilisateur décoche la préférence <code>Déplacer ou redimensionner des fenêtres existantes</code>. Déplacer et redimensionner une fenêtre à distance sur l'écran de l'utilisateur l'ennuiera très souvent, le désorientera, et au mieux sera incorrect. Les auteurs Web espèrent souvent avoir un contrôle absolu (et un pouvoir de décision) sur tous les aspects de positionnement et de taille des fenêtres de l'utilisateur ... ce qui n'est tout simplement pas vrai.</p></dd>
 <dt>Comment ouvrir une ressource référencée par un lien dans un nouvel onglet ? ou dans un onglet spécifique ?</dt>
 <dd>Pour l'instant, ce n'est pas possible. Seul l'utilisateur peut modifier ses préférences avancées pour faire cela. <a href="http://kmeleon.sourceforge.net/">K-meleon 1.1</a>, un navigateur basé sur Mozilla, donne un contrôle et un pouvoir complet à l'utilisateur sur la manière dont les liens sont ouverts. Certaines extensions avancées donnent également à Mozilla et Firefox un grand pouvoir concernant la manière dont les ressources référencées sont chargées. Dans quelques années, la <a href="http://www.w3.org/TR/2004/WD-css3-hyperlinks-20040224/#target0">propriété target du module CSS3 hyperlien</a> pourrait être implémentée (si le module CSS3 Hyperlink tel qu'il existe à présent est approuvé). Même si cela se fait et lorsque cela se produira, attendez-vous à ce que les développeurs de navigateurs utilisant des onglets donnent un pouvoir de veto à l'utilisateur et un contrôle total de la façon dont les liens peuvent ouvrir des pages Web. La façon d'ouvrir un lien devrait toujours être entièrement sous le contrôle de l'utilisateur.</dd>
 <dt>Comment savoir si une fenêtre que j'ai ouverte l'est toujours ?</dt>
 <dd>Vous pouvez tester l'existence de la référence à l'objet <code>window</code>, qui est la valeur renvoyée en cas de succès de l'appel à <code>window.open()</code>, et vérifier ensuite que la valeur renvoyée par <var>WindowObjectReference</var>.closed est bien <var>false</var>.</dd>
 <dt>Comment savoir si ma fenêtre a été bloquée par un bloqueur de popups ?</dt>
 <dd>Avec les bloqueurs de popups intégrés dans Mozilla/Firefox et Internet Explorer 6 SP2, il est possible de vérifier la valeur renvoyée par <code>window.open()</code> : elle sera <var>null</var> si la fenêtre n'a pas été autorisée à s'ouvrir. Cependant, pour la plupart des autres bloqueurs de popups, il n'existe pas de manière fiable.</dd>
 <dt>Quelle est la relation JavaScript entre la fenêtre principale et la fenêtre secondaire ?</dt>
 <dd>La valeur renvoyée par la méthode <code>window.open()</code> est la propriété <a href="fr/DOM/window.opener">opener</a>. La variable <var>WindowObjectReference</var> lie la fenêtre principale (opener) à la fenêtre secondaire qu'elle a ouverte, tandis que le mot-clé <code>opener</code> liera la fenêtre secondaire à sa fenêtre principale (celle qui a déclenché son ouverture).</dd>
 <dt>Je n'arrive pas à accéder aux propriétés de la nouvelle fenêtre secondaire. J'obtiens toujours une erreur dans la console JavaScript disant « Erreur : uncaught exception: Permission denied to get property &lt;property_name or method_name&gt; ». Pourquoi cela ?</dt>
 <dd><p>Ceci est causé par la restriction de sécurité des scripts entre domaines (aussi appelée<em>Same Origin Policy</em> , « Politique de même origine »). Un script chargé dans une fenêtre (ou cadre) d'une origine donnée (nom de domaine) <strong>ne peut pas obtenir ou modifier</strong> des propriétés d'une autre fenêtre (ou cadre) ou les propriétés d'aucun de ses objets HTML si celle-ci provient d'une autre origine distincte (nom de domaine). C'est pourquoi, avant d'exécuter un script se référant à une fenêtre secondaire depuis la fenêtre principale, le navigateur vérifiera que la fenêtre secondaire possède le même nom de domaine. Plus d'informations à propos de la restriction de sécurité des scripts entre domaines : <a href="http://www.mozilla.org/projects/security/components/same-origin.html">http://www.mozilla.org/projects/secu...me-origin.html</a></p></dd>
</dl>

<h3 id="Probl.C3.A8mes_d.27utilisabilit.C3.A9">Problèmes d'utilisabilité</h3>

<h4 id=".C3.89vitez_de_recourir_.C3.A0_window.open.28.29">Évitez de recourir à <code>window.open()</code></h4>

<p>De manière générale, il est préférable d'éviter d'utiliser <code>window.open()</code> pour plusieurs raisons :</p>

<ul>
 <li>Tous les navigateurs basés sur Mozilla offrent la navigation par onglets, et il s'agit du mode préféré pour ouvrir des ressources référencées… pas seulement dans le cas des navigateurs basés sur Mozilla, mais dans tous les autres navigateurs offrant la navigation par onglets. En d'autres mots, les utilisateurs de navigateurs utilisant des onglets préfèrent ouvrir des onglets que des fenêtres dans la plupart des situations. Ce type de navigateur gagne rapidement en popularité depuis plusieurs années et cette tendance ne semble pas près de s'arrêter. La version 7 d'Internet Explorer sortie en octobre 2006 propose également la navigation par onglets.</li>
 <li>Il existe à présent <a href="https://addons.update.mozilla.org/extensions/showlist.php?application=mozilla&amp;category=Tabbed+Browsing&amp;numpg=50&amp;os=windows&amp;version=auto-detect&amp;submit=Update">plusieurs extensions à Mozilla</a> (comme Multizilla) et <a href="https://addons.update.mozilla.org/extensions/showlist.php?application=firefox&amp;version=1.0+&amp;os=Windows&amp;category=Tabbed%20Browsing">à Firefox</a> (comme <a href="https://addons.update.mozilla.org/extensions/moreinfo.php?application=firefox&amp;version=1.0%20&amp;os=Windows&amp;category=Tabbed%20Browsing&amp;numpg=10&amp;id=158">Tabbrowser preferences</a>), fonctionnalités et préférences avancées basées sur la navigation par onglets, sur la conversion des appels à <code>window.open()</code> en ouvertures d'onglets, et sur la neutralisation des appels à <code>window.open()</code>. En particulier, afin de neutraliser l'ouverture de nouvelles fenêtres non demandées (souvent présentés comme bloquant les fenêtre popups non sollicitées ou les ouvertures automatiques de fenêtres par des scripts). Parmi ces fonctionnalités qu'on peut retrouver dans des extensions, il y a l'ouverture d'un lien dans une nouvelle fenêtre ou non, dans la même fenêtre, dans un nouvel onglet ou non, en arrière-plan ou non. Coder sans réfléchir pour ouvrir de nouvelles fenêtres n'est plus assuré de succès, ne pourra pas se faire par la force, et dans le cas où l'auteur Web y parvient, ne fera qu'ennuyer la majorité des utilisateurs.</li>
 <li>Les nouvelles fenêtres peuvent avoir leur barre de menu, leurs barres de défilement, leur barre d'état, leur redimensionnabilité etc. désactivées. Ceci n'est pas possible avec de nouveaux onglets. Par conséquent, de nombreux utilisateurs préfèrent utiliser des onglets étant donné que l'interface de leur navigateur est laissée intacte et reste stable.</li>
 <li>L'ouverture de nouvelles fenêtres, même avec leurs fonctionnalités réduites, utilise des ressources système considérables sur l'ordinateur de l'utilisateur (processeur, mémoire RAM) et met en jeu une grande quantité de code à exécuter (gestion de la sécurité, de la mémoire, diverses options de code parfois assez complexes, la construction du cadre de la fenêtre, des barres d'outils de la fenêtre, son positionnement et sa taille, etc.). L'ouverture de nouveaux onglets demande nettement moins de ressources système et est plus rapide à exécuter que d'ouvrir de nouvelles fenêtres.</li>
</ul>

<h4 id="Offrez_d.27ouvrir_un_lien_dans_une_nouvelle_fen.C3.AAtre.2C_en_suivant_ces_recommandations">Offrez d'ouvrir un lien dans une nouvelle fenêtre, en suivant ces recommandations</h4>

<p>Si vous voulez permettre l'ouverture d'un lien dans une nouvelle fenêtre, suivez ces règles d'utilisabilité et d'accessibilité testées et éprouvées :</p>

<h5 id="N.27utilisez_jamais_ce_format_de_code_pour_les_liens_.3Ca_href.3D.22javascriptwindow.open.28....29.22_....3E">N'utilisez <em>jamais</em> ce format de code pour les liens :<br>
 <code>&lt;a href="javascript:window.open(...)" ...&gt;</code></h5>

<p>Les liens « javascript: » sont toujours mauvais pour l'accessibilité et l'utilisabilité des pages Web dans tous les navigateurs.</p>

<ul>
 <li>Les pseudo-liens « javascript:» ne fonctionnent plus du tout lorsque la gestion de JavaScript est désactivée ou inexistante. Certaines sociétés n'autorisent leurs employés à utiliser le Web que suivant des politiques de sécurité très strictes : JavaScript désactivé, pas de Java, pas d'ActiveX, pas de Flash. Pour diverses raisons (sécurité, accès public, navigateurs texte, etc.), environ 5% à 10% des utilisateurs naviguent sur le Web avec JavaScript désactivé.</li>
 <li>Les liens « javascript: » interfèrent avec les fonctionnalités avancées de la navigation par onglets : entre autres, le clic du milieu sur des liens, le raccourci Ctrl+clic sur un lien, les fonctions de certaines extensions, etc.</li>
 <li>Les liens « javascript: » interfèrent avec le processus d'indexation des pages Web par les moteurs de recherche.</li>
 <li>Les liens « javascript: » interfèrent avec les technologies d'assistance (par exemple les lecteurs vocaux) et diverses applications utilisant le Web (par exemple les <abbr>PDA</abbr> ou les navigateurs pour mobiles).</li>
 <li>Les liens « javascript: » interfèrent également avec les fonctionnalités de « mouse gestures » proposées par certains navigateurs.</li>
 <li>Le schéma de protocole « javascript: » sera rapporté comme une erreur par les validateurs et vérificateurs de liens.</li>
</ul>

<p><strong>Plus d'informations à ce sujet :</strong></p>

<ul>
 <li><a href="http://www.useit.com/alertbox/20021223.html">Top Ten Web-Design Mistakes of 2002</a>, 6. JavaScript in Links, Jakob Nielsen, Décembre 2002</li>
 <li><a href="http://www.evolt.org/article/Links_and_JavaScript_Living_Together_in_Harmony/17/20938/">Links &amp; JavaScript Living Together in Harmony</a>, Jeff Howden, Février 2002</li>
 <li><a href="http://jibbering.com/faq/#FAQ4_24">FAQ de la discussion dans le newsgroup comp.lang.javascript sur les liens "javascript:"</a></li>
</ul>

<h5 id="N.27utilisez_jamais_.3Ca_href.3D.22.23.22_onclick.3D.22window.open.28....29.3B.22.3E">N'utilisez jamais <code>&lt;a href="#" onclick="window.open(...);"&gt;</code></h5>

<p>Un tel pseudo-lien met également en péril l'accessibilité des liens. <strong>Utilisez toujours une véritable URL pour la valeur de l'attribut href</strong>, afin que si JavaScript s'avère désactivé ou absent, ou si le navigateur ne peut pas ouvrir de fenêtre secondaire (comme Microsoft Web TV, les navigateurs en mode texte, etc.), de tels navigateurs pourront toujours charger la référence référencée dans leur mode de chargement/gestion de nouvelles ressources par défaut. Cette forme de code interfère également avec les fonctionnalités avancées de navigation par onglets de certains navigateurs, comme le clic du milieu et les raccourcis Ctrl+clic et Ctrl+Entrée sur les liens, les « mouse gestures », etc.</p>

<h5 id="Identifiez_toujours_les_liens_qui_cr.C3.A9eront_.28ou_r.C3.A9utiliseront.29_une_nouvelle_fen.C3.AAtre_secondaire">Identifiez toujours les liens qui créeront (ou réutiliseront) une nouvelle fenêtre secondaire</h5>

<p>Identifiez les liens qui ouvriront de nouvelles fenêtre de manière à aider les utilisateurs en utilisant l'attribut <code>title</code> du lien, en ajoutant une icône à la fin du lien, ou en modifiant le curseur de la souris.</p>

<p>Le but est d'avertir les utilisateurs à l'avance des changements de contexte pour réduire la confusion de leur part : changer la fenêtre courante ou afficher subitement une nouvelle fenêtre peut être très désorientant pour certains utilisateurs (le bouton Précédent devient inutilisable).</p>

<blockquote>
<p>« Souvent, les utilisateurs ne remarquent pas qu'une nouvelle fenêtre s'est ouverte, particulièrement si ils utilisent un petit écran où les fenêtres sont agrandies pour utiliser toute la surface de l'écran. Ainsi, l'utilisateur voulant retourner à la page initiale sera dérouté par un bouton<em>Précédente</em> grisé. »<br>
 citation de <a href="http://www.useit.com/alertbox/990530.html">The Top Ten<em>New</em> Mistakes of Web Design</a>: 2. Opening New Browser Windows, Jakob Nielsen, mai 1999</p>
</blockquote>

<p>Lorsque des changements complets de contexte sont identifiés explicitement avant qu'ils se produisent, l'utilisateur peut déterminer s'il désire le faire ou s'y préparer : non seulement il ne sera pas perturbé ni désorienté, mais des utilisateurs plus expérimentés pourront eux-mêmes décider comment ouvrir de tels liens (dans une nouvelle fenêtre ou non, dans la même fenêtre, dans un nouvel onglet ou non, en arrière-plan ou non).</p>

<p><strong>Références</strong></p>

<ul>
 <li>« Si votre lien crée une nouvelle fenêtre, ou fait apparaitre d'autres fenêtres sur votre affichage, ou déplace le focus du système vers un autre cadre ou une autre fenêtre, alors la chose sympathique à faire est d'indiquer à l'utilisateur que quelque chose de ce genre va se produire. » <a href="http://www.w3.org/WAI/wcag-curric/sam77-0.htm">World Wide Web Consortium Accessibility Initiative regarding popups</a></li>
 <li>« Utilisez les titres de liens pour donner aux utilisateurs un aperçu d'où chaque lien les emmènera, avant qu'ils aient cliqué dessus. » <a href="http://www.useit.com/alertbox/991003.html">Ten Good Deeds in Web Design</a>, Jakob Nielsen, octobre 1999</li>
 <li><a href="http://www.useit.com/alertbox/980111.html">Using Link Titles to Help Users Predict Where They Are Going</a>, Jakob Nielsen, janvier 1998</li>
</ul>

<h5 id="Utilisez_toujours_l.27attribut_target">Utilisez toujours l'attribut target</h5>

<p>Si JavaScript est désactivé ou absent, le navigateur créera une fenêtre secondaire ou affichera la ressource demandée selon sa gestion habituelle de l'attribut target : par exemple certains navigateurs ne pouvant pas créer de nouvelles fenêtres comme Microsoft Web TV, chargeront la ressource demandée et l'ajouteront à la fin du document courant. Le but et l'idée est d'essayer de fournir — <strong>sans l'imposer</strong> — à l'utilisateur une manière d'ouvrir la ressource référencée, de suivre le lien. Votre code ne doit pas interférer avec les fonctionnalités du navigateur à disposition de l'utilisateur, et doit toujours lui laisser le chemin libre et le choix de la décision finale.</p>

<h5 id="N.27utilisez_pas_target.3D.22_blank.22">N'utilisez pas <code>target="_blank"</code></h5>

<p>Fournissez toujours un nom significatif dans votre attribut target, et essayez de le réutiliser dans votre page afin qu'un clic sur un autre lien puisse charger la ressource dans une fenêtre déjà utilisée (ce qui rend l'opération beaucoup plus rapide pour l'utilisateur), ce qui justifie la raison (et les ressources système, et le temps dépensé) d'avoir créé une fenêtre secondaire. Utiliser une seule cible et la réutiliser pour plusieurs liens est bien moins gourmand en matière de ressources puisqu'une seule fenêtre est créée et recyclée. D'un autre côté, utiliser « _blank » comme attribut target créera plusieurs nouvelles fenêtre anonymes sur le bureau de l'utilisateur qui ne peuvent ni être recyclées ni réutilisées.</p>

<p>Dans tous les cas, si votre code est bien fait, il ne devrait pas interférer avec le choix final de l'utilisateur mais lui offrira plutôt des choix supplémentaires, plus de façons d'ouvrir les liens et plus de pouvoir sur l'outil qu'il utilise (son navigateur).</p>

<h3 id="Glossaire">Glossaire</h3>

<dl>
 <dt>Fenêtre ouvrante, opener, fenêtre parente, fenêtre principale, première fenêtre</dt>
 <dd>Termes souvent utilisés pour décrire ou identifier la même fenêtre. Il s'agit de la fenêtre depuis laquelle une nouvelle fenêtre sera créée. C'est la fenêtre dans laquelle l'utilisateur clique sur un lien qui mène à la création d'une autre, nouvelle fenêtre.</dd>
 <dt>Sous-fenêtre, fenêtre enfant, fenêtre secondaire, deuxième fenêtre</dt>
 <dd>Termes souvent utilisés pour décrire ou identifier la même fenêtre. C'est la nouvelle fenêtre qui a été créée.</dd>
 <dt>Fenêtres popup non sollicitées</dt>
 <dd>Ouverture automatique de fenêtres initiée par un script sans le consentement de l'utilisateur.</dd>
</dl>

<h3 id="Sp.C3.A9cification">Spécification</h3>

<p>{{ DOM0() }} <code>window.open()</code> ne fait partie d'aucune spécification ou recommandation technique du <abbr>W3C</abbr>.</p>

<h3 id="Notes">Notes</h3>

<h4 id="Note_sur_les_priorit.C3.A9s">Note sur les priorités</h4>

<p>Dans les cas où <code>left</code> et <code>screenX</code> (et/ou <code>top</code> et <code>screenY</code>) ont des valeurs contradictoires, <code>left</code> et <code>top</code> ont priorité sur <code>screenX</code> et <code>screenY</code> respectivement. Si <code>left</code> et <code>screenX</code> (et/ou <code>top</code> et <code>screenY</code>) sont définies dans la liste <var>strWindowFeatures</var>, les valeurs <code>left</code> (et/ou <code>top</code>) seront reconnues et utilisées. Dans l'exemple suivant, la nouvelle fenêtre sera positionnée à 100 pixels du bord gauche de la zone de travail des applications du système d'exploitation de l'utilisateur, et non à 200 pixels.</p>

<pre class="brush: html">&lt;script type="text/javascript"&gt;
WindowObjectReference = window.open("http://www.lemonde.fr/",
           "NomDeLaFenetreLeMonde",
           "left=100,screenX=200,resizable,scrollbars,status");
&lt;/script&gt;
</pre>

<p>Si <code>left</code> est défini, mais que <code>top</code> n'a aucune valeur mais que <code>screenY</code> en a une, <code>left</code> et <code>screenY</code> seront les coordonnées de positionnement de la fenêtre secondaire.</p>

<p><code>outerWidth</code> a priorité sur <code>width</code>, et <code>width</code> a priorité sur <code>innerWidth</code>. <code>outerHeight</code> a priorité sur <code>height</code> et <code>height</code> a priorité sur <code>innerHeight</code>. Dans l'exemple suivant, les navigateurs basés sur Mozilla créeront une nouvelle fenêtre avec une taille extérieure (outerWidth) de 600 pixels de large et ignoreront la requête d'une largeur (width) de 500 pixels ainsi que celle d'une taille intérieure (innerWidth) de 400 pixels.</p>

<pre class="brush: html">&lt;script type="text/javascript"&gt;
WindowObjectReference = window.open("http://www.wwf.org/",
           "NomDeLaFenetreWWF",
           "outerWidth=600,width=500,innerWidth=400,resizable,scrollbars,status");
&lt;/script&gt;
</pre>

<h4 id="Note_sur_les_corrections_d.27erreurs_de_position_et_de_dimension">Note sur les corrections d'erreurs de position et de dimension</h4>

<p>Les positions et dimensions demandées dans la liste <var>strWindowFeatures</var> ne seront pas respectées et <strong>seront corrigées</strong> si n'importe laquelle de ces valeurs ne permet pas à la fenêtre complète d'être affichée dans la zone de travail des applications du système d'exploitation de l'utilisateur. <strong>Aucune partie de la nouvelle fenêtre ne peut être initialement positionnée hors de l'écran. Ceci est le réglage par défaut de tous les navigateurs basés sur Mozilla.</strong></p>

<p><a href="http://msdn.microsoft.com/security/productinfo/xpsp2/default.aspx?pull=/library/en-us/dnwxp/html/xpsp2web.asp#xpsp_topic5">Internet Explorer 6 SP2 dispose d'un mécanisme de correction d'erreur similaire</a>, mais celui-ci n'est pas activé par défaut pour tous les niveaux de sécurité : un niveau de sécurité trop bas peut désactiver ce mécanisme de correction d'erreurs.</p>

<h4 id="Note_sur_les_barres_de_d.C3.A9filement">Note sur les barres de défilement</h4>

<p>Lorsque le contenu dépasse les dimensions de la zone de visualisation de la fenêtre, des barres de défilement (ou un mécanisme de définement similaire) sont nécessaires pour s'assurer que les utilisateurs puissent accéder au contenu. Le contenu ou la boîte du document d'une page web peut outrepasser, excéder les dimensions demandées pour la fenêtre pour plusieurs raisons qui sont hors du contrôle des auteurs Web :</p>

<ul>
 <li>l'utilisateur a redimensionné la fenêtre</li>
 <li>l'utilisateur a augmenté la taille de police du texte via le menu Affichage/Taille du texte (%) dans Mozilla ou Affichage/Taille du texte/Plus grande ou Plus petite dans Firefox</li>
 <li>l'utilisateur a défini une taille minimale de police pour les pages qui est plus grande que celle demandée par l'auteur. Les personnes de plus de 40 ans ou ayant des habitudes particulières de lecture choisissent souvent une taille minimale de police dans les navigateurs basés sur Mozilla.</li>
 <li>l'auteur n'est pas conscient des marges par défaut (et/ou bordures et/ou padding) appliquées à l'élément racine ou nœud <code>body</code> dans les différents navigateurs et versions de ceux-ci</li>
 <li>l'utilisateur utilise une feuille de style utilisateur (<a href="http://www.mozilla.org/support/firefox/edit#content">userContent.css in Mozilla-based browsers</a>) pour ses habitudes de navigation qui modifie les dimensions habituelles pour ses habitudes de lecture (marges, padding, taille de police par défaut)</li>
 <li>l'utilisateur peut personnaliser individuellement la taille (hauteur ou largeur) de la plupart des barres d'outils via son système d'exploitation. Par exemple les bordures de fenêtres, la hauteur de la barre de titre, des menus et des barres de défilement, ainsi que la taille du texte sont entièrement personnalisables par l'utilisateur dans le système d'exploitation Windows XP. Ces dimensions de barres d'outils peuvent également être changées par des thèmes du navigateur, ou du système d'exploitation</li>
 <li>l'auteur ne s'attend pas à ce que la fenêtre de l'utilisateur dispose de barres d'outils spécifiques supplémentaires ; comme des barres de préférences, la barre web developer, des barres d'accessibilité, de blocage de popups et de recherche, etc.</li>
 <li>l'utilisateur utilise des technologies assistives ou addons qui modifient la zone de travail des applications, par exemple Microsoft Magnifier</li>
 <li>l'utilisateur repositionne et/ou redimensionne directement la zone de travail des apllications : par exemple il redimensionne la barre des tâches de Windows, la place sur le côté gauche de l'écran (versions en langues arabes) ou sur la droite (versions en hébreu), l'utilisateur a une barre de lancement rapide Microsoft Office, etc.</li>
 <li>certains systèmes d'exploitation (Mac OS X) forcent la présence de barres d'outils qui peuvent démentir les anticipations et calculs de l'auteur Web sur les dimensions effectives de la fenêtre de navigation</li>
</ul>

<h4 id="Note_sur_la_barre_d.27.C3.A9tat">Note sur la barre d'état</h4>

<p>Vous devez vous attendre à ce qu'une large majorité des navigateurs affichent la barre d'état, ou qu'une large majorité des utilisateurs voudront forcer sa présence : le mieux est de toujours positionner cette fonctionnalité à yes. De plus, si vous demandez spécifiquement de retirer la barre d'état, les utilisateurs de Firefox ne pourront plus utilser la barre de navigation de site si elle est installée. Dans Mozilla et dans Firefox, toutes les fenêtres peuvent être redimensionnées à partir de la zone inférieure droite de la barre d'état. Celle-ci fournit également des informations sur la connexion http, l'emplacement des ressources hypertextes, la barre de progression du téléchargement, l'icône de chiffrement/connexion sécurisée avec <abbr>SSL</abbr> (affichant une icône de cadenas jaune), des icônes de zones de sécurité, de politique de sécurité en matière de cookies, etc. <strong>Enlever la barre d'état retire souvent de très nombreuses fonctionnalités et informations considérées comme utiles (et parfois vitales) par les utilisateurs.</strong></p>

<h4 id="Note_sur_les_probl.C3.A8mes_de_s.C3.A9curit.C3.A9_li.C3.A9s_.C3.A0_la_pr.C3.A9sence_de_la_barre_d.27.C3.A9tat">Note sur les problèmes de sécurité liés à la présence de la barre d'état</h4>

<p>Dans Internet Explorer 6 pour XP SP2, pour les fenêtres ouvertes à l'aide de <code>window.open()</code> :</p>

<blockquote>
<p>« Pour les fenêtres ouvertes à l'aide de <code>window.open()</code> :<br>
 Attendez-vous à ce que la barre d'état soit présente, et codez en conséquence. <strong>La barre d'état sera activée par défaut</strong> et fait entre 20 et 25 pixels de haut. (...) »<br>
 citation de <a href="http://msdn.microsoft.com/security/productinfo/xpsp2/default.aspx?pull=/library/en-us/dnwxp/html/xpsp2web.asp#xpsp_topic5">Fine-Tune Your Web Site for Windows XP Service Pack 2, Browser Window Restrictions in XP SP2</a></p>
</blockquote>

<blockquote>
<p>« (...) les fenêtres créées à l'aide de la méthode <code>window.open()</code> peuvent être appelées par des scripts et utilisées pour imiter une fausse interface utilisateur ou bureau, ou pour masquer des informations ou des activités malveillantes en dimensionnant la fenêtre de manière à rendre la barre d'état invisible.<br>
 Les fenêtres d'Internet Explorer fournissent des informations de sécurité visibles par l'utilisateur pour l'aider à identifier avec certitude la source de la page et le niveau de sécurité de la communication avec cette page. Lorsque ces éléments ne sont pas visibles, les utilisateurs peuvent penser qu'ils sont sur une page plus digne de confiance ou interagissent avec un processus système alors qu'il sont en train d'interagir avec un hôte malveillant. (...)<br>
 <strong>Les fenêtres lancées par des scripts seront affichées entièrement, avec la barre de titre d'Internet Explorer et sa barre d'état.</strong> (...)<br>
 Gestion de la barre d'état d'Internet Explorer par les scripts<br>
 Description détaillée<br>
 <strong>Internet Explorer a été modifié pour ne désactiver la barre d'état pour aucune fenêtre. La barre d'état est toujours visible pour toutes les fenêtres d'Internet Explorer.</strong> (...) Sans ce changement, les fenêtres créées à l'aide de la méthode <code>window.open()</code> peuvent être appelées par des scripts et imiter une fausse interface utilisateur ou bureau, ou pour masquer des informations importantes pour l'utilisateur.<br>
 La barre d'état est une fonction de sécurité des fenêtres d'Internet Explorer qui fournissent à l'utilisateur des informations sur les zones de sécurité. Cette zone ne peut pas être imitée (...)"<br>
 citation de <a href="http://www.microsoft.com/technet/prodtechnol/winxppro/maintain/sp2brows.mspx#ECAA">Changes to Functionality in Microsoft Windows XP Service Pack 2, Internet Explorer Window Restrictions</a></p>
</blockquote>

<h4 id="Note_sur_le_plein_.C3.A9cran_.28fullscreen.29">Note sur le plein écran (fullscreen)</h4>

<p>Dans Internet Explorer 6 pour XP SP2 :</p>

<ul>
 <li>« <code>window.open()</code> avec fullscreen=yes donnera à présent une fenêtre maximisée, et non une fenêtre en mode kiosque. »</li>
 <li>« La définition de la spécification de fullscreen=yes est modifiée pour signifier "afficher la fenêtre maximisée," ce qui gardera la barre de titre, la barre d'adresse et la barre d'état visibles. »</li>
</ul>

<p><em>Références :</em></p>

<ul>
 <li><a href="http://msdn.microsoft.com/security/productinfo/xpsp2/default.aspx?pull=/library/en-us/dnwxp/html/xpsp2web.asp#xpsp_topic5">Fine-Tune Your Web Site for Windows XP Service Pack 2</a></li>
 <li><a href="http://www.microsoft.com/technet/prodtechnol/winxppro/maintain/sp2brows.mspx#ECAA">Changes to Functionality in Microsoft Windows XP Service Pack 2, Script sizing of Internet Explorer windows</a></li>
</ul>

<h4 id="Note_sur_la_diff.C3.A9rence_entre_outerHeight_et_height">Note sur la différence entre outerHeight et height</h4>


<h3 id="Tutoriels">Tutoriels</h3>

<p><strong>en français :</strong></p>

<p><a href="http://openweb.eu.org/articles/popup/">Créer des pop-up intelligentes</a> par Fabrice Bonny, OpenWeb</p>

<p><strong>en anglais :</strong></p>

<p><a href="http://www.infimum.dk/HTML/JSwindows.html">JavaScript windows (tutorial)</a> par Lasse Reichstein Nielsen</p>

<p><a href="http://www.accessify.com/tutorials/the-perfect-pop-up.asp">The perfect pop-up (tutorial)</a> par Ian Lloyd</p>

<p><a href="http://www.gtalbot.org/FirefoxSection/Popup/PopupAndFirefox.html">Popup windows and Firefox (interactive demos)</a> par Gérard Talbot</p>

<h3 id="R.C3.A9f.C3.A9rences">Références</h3>

<p><a href="http://www.cs.tut.fi/~jkorpela/www/links.html">Links Want To Be Links</a> by Jukka K. Korpela</p>

<p><a href="http://www.evolt.org/article/Links_and_JavaScript_Living_Together_in_Harmony/17/20938/">Links &amp; JavaScript Living Together in Harmony</a> by Jeff Howden</p>