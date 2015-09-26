
Codex Web

    Retour

Positionnement dans le flux : block, inline et cie

Les éléments html ont par défaut 2 grands modes de rendus : block et inline. Ils peuvent au besoin, être reclassés grâce à la propriété display.
Block

Un élément avec un rendu de type block se place toujours l'un en dessous de l'autre (comme un retour chariot). Ses propriétés :

    il occupe toute la largeur de son conteneur
    il permet l'attribution de marge de côté
    il permet la modification de sa largeur et sa hauteur

Ex de balises par défaut en mode rendu block : div, p, table, h1, etc..

Inline

Les élements avec un rendu inline se placent toujours l'un à côté de l'autre. Il n'est pas prévu qu'ils puissent se positionner sur la page, ni leur donner de dimensions. Leur taille va être déterminée par le texte ou l'élément qu'ils contiennent. On peut malgré tout mettre des padding ou margin left ou right (mais sans effet avec top ou bottom). Ex : span, a, img, b, etc..
Inline-block

Les élements avec un rendu inline-block ont les mêmes caractéristiques que les inline, mais ils peuvent être dimensionnés (p. ex. la balise input).
List-item

Les éléments avec rendu list-item ont un rendu de type block mais peuvent bénéficier des propriétés liées aux puces (list-style, etc..), par exemple la balise li
Table, table-row, table-cell

Les éléments avec rendu table, table-row, table-cell ont le même comportement que les tableaux et cellules. Ils permettent de centrer les contenus verticalement et d'avoir des hauteurs identiques entre éléments frères (p. ex. la balise td).
Voici les éléments html les plus importants et leur mode de rendu par défaut (CSS 2.1).

		html, address,
		blockquote,
		body, dd, div,
		dl, dt, fieldset, form,
		frame, frameset,
		h1, h2, h3, h4,
		h5, h6, noframes,
		ol, p, ul, center,
		dir, hr, menu, pre   { display: block }

		li              { display: list-item }
		head            { display: none }
		table           { display: table }
		tr              { display: table-row }
		thead           { display: table-header-group }
		tbody           { display: table-row-group }
		tfoot           { display: table-footer-group }
		col             { display: table-column }
		colgroup        { display: table-column-group }

		td, th          { display: table-cell }
		caption         { display: table-caption }
		input, select   { display: inline-block }

	

Position
Relative

Permet d'inscrire un élément dans le flux normal puis de le décaler horizontalement ou verticalement. Peu utilisé. Peu servir de référence pour un élément enfant qui serait en absolute. Permet d'utiliser le z-index
Absolue

S'affranchit totalement du flux, il n'existe plus pour les autres éléments qui sont restés dans le flux. La référence utilisé est son 1er ancêtre positionné (en absolu ou relatif), puis left, right, bottom, top pour le placer. Perd la capacité de prendre toute la largeur de l'élément parent et ne prend que la largeur suffisante pour afficher son contenu.
float

Une boîte flottante est retirée du flux normal et placée le plus à droit ou à gauche possible dans son conteneur. Le contenu suivant cette boîte s'écoule le long de celle-ci dans l'espace laissé libre.

Perd la capacité de prendre toute la largeur de l'élément parent et ne prend que la largeur suffisante pour afficher son contenu.

Pour que l'habillage apporté par les flottants s'arrête, il faut utiliser la propriété clear (avec les valeurs : left, right ou both).
Ex avec un élément invisible hr :

		hr {
		  clear: both;
		  visibility: hidden;
		}
	

Modèle de boîte

Lorsqu'on attribue une taille à un élément de type block, à l'aide d'un widht ou d'un height, les marges viennents s'ajouter à cette taille. Idem pour le padding.

En fait, le width total est égal à : width + padding-left + padding-right + border-left + border-right. Même raisonnement pour le height. Le width CSS est la largeur du contenu, à l'intérieur des borders et du padding... C'est pourquoi il est important de faire un reset CSS car chaque navigateur a ses propres valeurs par défaut pour padding et width.

Par défaut, le width est à une sorte de 100%, mais plus dans le sens : prendre ce qu'il reste. Si on met explicitment 100%, il utilisera vraiment les 100% du parent.

Pour éviter cela, on peut :

    modifier le box model ,en mettant box-sizing: border-box (ne fonctionne pas pour IE7 et inférieur).

    				-moz-box-sizing: border-box;
    				-ms-box-sizing: border-box;
    				-o-box-sizing: border-box;
    				-webkit-box-sizing: border-box;
    				box-sizing: border-box;
            		

Liens :

    Positionnement float Openweb
    Positionnement par Alsacreations

