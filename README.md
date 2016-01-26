<h1>Documentation Plugin Home Center de Fibaro pour SARAH</h1><br />
<p>
Auteur : e1000 (code inspiré du plugin Zibase de Baptiste MARIENVAL)<br />

<p><i>Version 0.3.2 (26/01/2016)</i></p>

1. CONFIGURATION : homecenter.prop<br />
2. MODULES : devices.xml<br />
3. ACTIONS : homecenter.xml<br />
4. COMMENT ÇA MARCHE ?<br />

<h2>1. CONFIGURATION : <tt>homecenter.prop</tt></h2>

<p>Ce fichier contient les éléments indispensables pour la connexion à votre box Home Center de Fibaro :</p>

<ul>
<li><b>url</b></li>
<p>Adresse IP ou nom de domaine sur lequel votre Home Center est accessible (ex.: 192.168.1.12).<br />
Pour connaître l'adresse IP de votre box, rendez-vous dans la partie <strong>Configuration</strong> puis <strong>Paramètres LAN</strong>.</p>
<img style="width:1000px;" src="./plugins/homecenter/images/configuration_reseau.png" />

<li><b>port</b></li>
<p>Port utilisé par le serveur de la box Home Center. Par défaut, le port 80 est utilisé.</p>

<li><b>username</b></li>
<p>Identifiant de l'utilisateur utilisé pour contrôle votre Home Center à distance.
Il est conseillé de créer un compte dédié à cet usage, et de ne pas utiliser le compte Admin.</p>

<li><b>password</b></li>
<p>Le mot de passe correspondant au compte ci-dessus.</p>
</ul>

<h2>2. MODULES : <tt>devices.xml</tt></h2>

<p>Tous les périphériques que vous souhaitez contrôler via SARAH sont définis dans le fichier <tt>devices.xml</tt>.</p>

<p>La configuration se veut la plus simple possible et ne nécessite que 4 types d'informations.<br />
À terme, la génération de ce fichier pourra être faite automatiquement.</p>
<p>Exemple :</p>
<pre>
&lt;devices&gt;
	&lt;!-- SONDES --&gt;
	&lt;device name="temp_salon" 		type="com.fibaro.temperatureSensor"	id="14" 	tts="La température du salon est de %s degrés " /&gt;
	&lt;device name="hygro_sdb" 		type="com.fibaro.humiditySensor" 	id="55" 	tts="L'hygrométrie de la salle de bain est de %s pourcent " /&gt;
	&lt;device name="lumi_entree" 		type="com.fibaro.lightSensor" 		id="132" 	tts="La luminosité de l'entrée est de %s pourcent " /&gt;
	&lt;!-- DETECTEURS --&gt;
	&lt;device name="mouvement_sam" 		type="com.fibaro.motionSensor" 		id="9" 		tts="Mouvement dans la salle à manger " /&gt;
	&lt;device name="ouverture_2" 		type="com.fibaro.doorSensor" 		id="96" 	tts="Capteur de porte 2 ouvert " /&gt;
	&lt;device name="fumee_chaufferie" 	type="com.fibaro.FGSS001" 		id="218" 	tts="Il y a de la fumée dans la chaufferie " /&gt;
	&lt;device name="chaleur_combles" 		type="com.fibaro.heatDetector" 		id="227" 	tts="Température anormale dans les combles " /&gt;
	&lt;device name="inondation_buanderie" 	type="com.fibaro.FGFS101" 		id="186"	tts="Inondation dans la buanderie " /&gt;
	&lt;!-- com.fibaro.binarySwitch --&gt;
	&lt;device name="lampe_cuisine" 		type="com.fibaro.binarySwitch" 		id="39" 	tts=" la lampe de la cuisine " /&gt;
	&lt;device name="machine_a_laver" 		type="com.fibaro.binarySwitch" 		id="158" 	tts=" la machine à laver " /&gt;
	&lt;!-- com.fibaro.multiLevelSwitch --&gt;
	&lt;device name="lampe_chambre" 		type="com.fibaro.multiLevelSwitch" 	id="80" 	tts=" la lampe de la chambre " /&gt;
	&lt;!-- VIRTUELS --&gt;
	&lt;device name="volet_tous" 		type="virtual_device" 			id="102" 	tts=" tous les volets "		buttonOn="1" buttonMy="2" buttonOff="3" slider="4" /&gt;
	&lt;device name="lampe_escalier" 		type="virtual_device" 			id="69" 	tts=" la lampe de l'escalier "	buttonOn="1" buttonOff="2" /&gt;
&lt;/devices&gt;
</pre>

Pour chaque élément <tt>&lt;device ... /&gt;</tt>, ont trouve :
<ul>
	<li><b>name</b></li> 
	<p><b>Nom du module</b> qui doit correspondre au nom utilisé pour les actions associées définies dans le fichier <tt>homecenter.xml</tt> décrit plus bas.<br />
	Ce nom doit être unique, mais ne doit pas obligatoirement correspondre au nom enregistré dans l'interface Home Center.<p>
	<li><b>type</b></li> 
	<p><b>Type de module</b> tel qu'ils sont définis dans l'API Fibaro. Il s'agit bien du <tt>type</tt> et non du <tt>basetype</tt>. <br />
	Ce paramètre est utilisé pour connaître les actions possibles sur ce module.
	</p>
	<li><b>id</b></li>
	<p>L'<b>identifiant</b> Home Center du module.</p>
	<li><b>tts</b></li>
	<p>Le <b>texte</b> que doit prononcer SARAH en retour lorsque l'action est réalisée. Pour les sondes, la chaine <tt>%s</tt> sera remplacée par la valeur retournée.</p>
</ul>
Dans le cas des modules virtuels uniquement, on trouve des paramètres supplémentaires :
<ul>
	<li><b>buttonOn</b></li>	
	<p>Ce paramètre précise le numéro du bouton à presser pour activer le module via la commande <tt>pressButton</tt>
	en remplacement de la commande <tt>turnOn</tt>.
	<li><b>buttonOff</b></li>	
	<p>Ce paramètre précise le numéro du bouton à presser pour désactiver le module via la commande <tt>pressButton</tt>
	en remplacement de la commande <tt>turnOff</tt>.
	<li><b>buttonMy</b></li>	
	<p>Spécifique aux volets roulants Somfy, ce paramètre précise le numéro du bouton à presser pour mettre le volet en position « My ».
	<li><b>slider</b></li>	
	<p>Ce paramètre précise le numéro du slider permettant de régler le module via la commande <tt>setSlider</tt>
	en remplacement de la commande <tt>setValue</tt>.
</ul>

<p>Voici la liste (non exhaustive) des types de modules :</p>
<ul>
<li>Capteurs ou sondes :</li>
<dl>
	<dt>com.fibaro.multilevelSensor</dt>
	<dd>Capteur générique</dd>
	<dt>com.fibaro.lightSensor</dt>
	<dd>Capteur de luminosité</dd>
	<dt>com.fibaro.temperatureSensor</dt>
	<dd>Capteur de température</dd>
	<dt>com.fibaro.humiditySensor</dt>
	<dd>Capteur d'humidité</dd>
</dl>
<li>Actionneurs :</li>
<dl>
	<dt>com.fibaro.binarySwitch</dt>
	<dd>Actionneur tout ou rien (On/Off)</dd>
	<dt>com.fibaro.multilevelSwitch</dt>
	<dd>Gradateur (ou <i>Dimmer</i>)</dd>
</dl>
<li>Détecteurs de sécurité :</li>
<dl>
	<dt>com.fibaro.motionSensor</dt>
	<dd>Détecteur de mouvement. Les périphériques esclaves de certains modules sont de ce type (exemple : capteur d'inondation FGFS101)</dd>
	<dt>com.fibaro.FGFS101</dt>
	<dd>Détecteur d'inondation</dd>
	<dt>com.fibaro.FGSS001</dt>
	<dd>Détecteur de fumée ; à noter que les détecteurs de deuxième génération (FGSD) sont également identifiés sous ce type</dd>
	<dt>com.fibaro.heatDetector</dt>
	<dd>Détecteur de chaleur anormalement élevée</dd>
	<dt>com.fibaro.doorSensor</dt>
	<dd>Détecteur d'ouverture</dd>
</dl>
<li>Divers :</li>
<dl>
	<dt>HC_user :</dt>
	<dd>Utilisateur de la box</dd> 
	<dt>weather :</dt>
	<dd>Informations météo</dd>
	<dt>iOS_device</dt>
	<dd>Téléphone ou tablette iOS</dd>
	<dt>virtual_device</dt>
	<dd>Périphérique virtuel</dd>
</dl>
</ul>

<h2>3. ACTIONS : <tt>homecenter.xml</tt></h2>

<p>Ce fichier contient l'ensemble de la grammaire et les actions à réaliser.<br />
Quelques exemples :</p>
<pre>
	<font color="blue">
	&lt;one-of&gt;
		&lt;item&gt;ouvre		&lt;tag&gt;out.action.actionModule="turnOn";	out.action.ttsAction="j'ouvre";	out.action.button="1";	&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;ferme		&lt;tag&gt;out.action.actionModule="turnOff";	out.action.ttsAction="je ferme";out.action.button="3";	&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;allume		&lt;tag&gt;out.action.actionModule="turnOn";	out.action.ttsAction="j'allume";	&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;éteint		&lt;tag&gt;out.action.actionModule="turnOff";	out.action.ttsAction="j'éteint";	&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;ferme		&lt;tag&gt;out.action.actionModule="turnOff";	out.action.ttsAction="je ferme";	&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;augmente		&lt;tag&gt;out.action.actionModule="setValue";out.action.ttsAction="oké, je règle";	&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;quelle est	&lt;tag&gt;out.action.actionModule="getValue";out.action.ttsAction="";		&lt;/tag&gt;&lt;/item&gt;
	&lt;/one-of&gt; 
	</font><font color="green">
	&lt;one-of&gt;
		&lt;!-- VOLETS --&gt;
		&lt;item&gt;le volet de la salle a manger	&lt;tag&gt;out.action.module="volet_sam";&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;le volet du séjour		&lt;tag&gt;out.action.module="volet_sejour";&lt;/tag&gt;&lt;/item&gt;
		&lt;!-- LAMPES --&gt;
		&lt;item&gt;la cuisine			&lt;tag&gt;out.action.module="lampe_cuisine";&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;la lampe de la cuisine		&lt;tag&gt;out.action.module="lampe_cuisine";&lt;/tag&gt;&lt;/item&gt;
		&lt;item&gt;la lumière de la cuisine		&lt;tag&gt;out.action.module="lampe_cuisine";&lt;/tag&gt;&lt;/item&gt;
		&lt;!-- Sondes --&gt;
		&lt;item&gt;la température du salon		&lt;tag&gt;out.action.module="temp_salon";&lt;/tag&gt;&lt;/item&gt;	
		&lt;item&gt;l'hygrométrie de la salle de bain	&lt;tag&gt;out.action.module="hygro_sdb";&lt;/tag&gt;&lt;/item&gt;	
		&lt;item&gt;la luminosité de l'entrée		&lt;tag&gt;out.action.module="lumi_entree";&lt;/tag&gt;&lt;/item&gt;	
	&lt;/one-of&gt;
	</font><font color="red">
	&lt;item repeat="0-1"&gt;
		&lt;one-of&gt;
			&lt;item&gt;à vingt cinq pour cent		&lt;tag&gt;out.action.ttsDim=" à vingt cinq pour cent";	out.action.setValue=25;&lt;/tag&gt;&lt;/item&gt;
			&lt;item&gt;à cinquante pour cent		&lt;tag&gt;out.action.ttsDim=" à cinquante pour cent";	out.action.setValue=50;&lt;/tag&gt;&lt;/item&gt;
			&lt;item&gt;à soixante quinze pour cent	&lt;tag&gt;out.action.ttsDim=" à soixante quinze pour cent";	out.action.setValue=75;&lt;/tag&gt;&lt;/item&gt;
			&lt;item&gt;à cent pour cent			&lt;tag&gt;out.action.ttsDim=" à cent pour cent";		out.action.setValue=100;&lt;/tag&gt;&lt;/item&gt;
		&lt;/one-of&gt;
	&lt;/item&gt;
	</font>
</pre>

<p>Les informations importantes de ce fichier sont :</p>

<ul>
<li><b>actionModule</b></li>	
<p>Action à réaliser ; les choix possibles sont : 
	<ul>
		<li>Commandes de type tout-ou-rien (<tt>turnOn</tt> et <tt>turnOff</tt>) :</li>  
			<p>Utilisée pour allumer ou éteindre les lumières, activer ou désactiver les prises commandées, ouvrir ou fermer les volets roulants, etc.</p>
			<p>Pour les périphériques à sortie variable tels que les gradateurs ou les commandes de volets roulants,
			et si la commande comporte une information de niveau (exemple : <i>« à 50 % »</i>) 
			ce paramètre est ignoré au bénéfice de <tt>setValue</tt> (cf ci-dessous).</p>
		<li>Commande de réglage (<tt>setValue</tt>) :</li> 
			<p>Utilisée pour régler l'intensité d'une lampe ou la position d'un volet roulant.</p>
			<p>Si vous omettez de préciser le réglage, cette action sera remplacée par <tt>turnOn</tt>.</p>
		<li>Commande de lecture (<tt>getValue</tt>) :</li> 
			<p>Utilisé pour lire la donnée d'une sonde, l'état d'un module, connaître la météo, etc.</p>
	</ul>
	
<li><b>ttsAction</b></li>      
<p>Phrase prononcée par SARAH lorsque l'action est réalisée.</p>

<li><b>module</b></li>         
<p>Le nom du module concerné par l'action. Ce nom doit correspondre à une (et une seule) des lignes <tt>&lt;device&gt;</tt> du fichier <tt>devices.xml</tt>.</p>

<li><b>setValue</b></li>
<p>Valeur à transmettre au module à sortie variable (dimmer, volet-roulant, etc.).<br />
Cette valeur est comprise en 0 et 100 %.</p>

<li><b>ttsDim</b></li>
<p>Phrase prononcée par SARAH pour confirmer le réglage.</p>
</ul>

<h2>4. COMMENT ÇA MARCHE ?</tt></h2>

<p>Dans le code de la section précédente, 3 blocs sont mis en évidence. 
Nous allons les illustrer avec la commande suivante :<br />
« <i> Sarah, allume la lampe de la cuisine à 50 %</i> ».</p>
<ul>
	<li>La description des <b>ordres</b> (en bleu)</li>
		<p>La première partie, juste après la balise <tt>&lt;item&gt;</tt>, 
		détaille la phrase à prononcer (toujours précédée de "Sarah", pour rappel).<br />
		Exemple : <i>Sarah, allume ...</i> va activer la ligne suivante :</p>
		<font color="blue">
		<pre>&lt;item&gt;allume		&lt;tag&gt;out.action.actionModule="turnOn";	out.action.ttsAction="j'allume";&lt;/tag&gt;&lt;/item&gt;</pre>
		</font>
		<p>À ce stade, SARAH sait qu'il faut allumer (car <tt>actionModule="turnOn"</tt>) 
		un module, mais ne sait pas encore lequel. 
		Elle mémorise également le début de sa réponse <tt>tts</tt> 
		(grâce à <tt>ttsAction=</tt><i>"j'allume"</i>)</p>

	<li>Le <b>module</b> à contrôler (en vert)</li>
		<p>La première partie constitue le complément de la phrase à prononcer.<br />
		Exemple : <i>...la cuisine</i> ou <i>... la lampe de la cuisine</i> ou encore
		<i>...la lumière de la cuisine</i>.<br /></p>
		<p>Dans l'exemple précédent, il y a ainsi trois façon de commander le même 
		dispositif. C'est l'avantage de la méthode manuelle qui consiste à adapter les 
		fichiers <tt>.xml</tt> pour que l'interaction avec SARAH soit la plus naturelle
		possible.</p>
		<font color="green">
		<pre>&lt;item&gt;la lampe de la cuisine		&lt;tag&gt;out.action.module="lampe_cuisine";&lt;/tag&gt;&lt;/item&gt;</pre>
		</font>
		<p>Maintenant, SARAH sait que le module à commander est celui identifié par
		<tt>module="lampe_cuisine"</tt>.<br />
		Elle sait donc qu'il faut « <tt>turnOn</tt> » le module « <tt>lampe_cuisine</tt>»
		c'est à dire « allumer la lampe de la cuisine »</p>
		<p>SARAH va donc interroger le fichier <tt>devices.xml</tt> pour chercher 
		l'<tt>id</tt> associé à <tt>lampe_cuisine</tt> (ici <tt>id="39"</tt>) et pourra 
		donc formatter l'url destinée à votre Home Center :<p>
		<pre>http://192.168.1.12:80/api/devices/39/action/turnOn</pre>
		<p>Elle va également récupérer le paramètre <tt>tts=" la lampe de la cuisine"</tt>
		afin de compléter sa réponse : <br />
		<i>j'allume la lampe de la cuisine</i></p>
		<p>La commande pourrait s'arrêter là, et tout fonctionnerait parfaitement. Mais en 
		fait, il est encore possible de compléter l'ordre donné à SARAH pour préciser le 
		degré d'allumage de la lampe : ici, 50 %.

	<li>Le <b>paramètre optionnel</b> (en rouge)</li>
		<p>Vous en avez maintenant l'habitude, la première partie permet de compléter la
		commande, c'est-à-dire la phrase prononcée à l'intention de SARAH.</p>
		<p>Avant tout, le premier élément important est le paramètre <tt>repeat</tt>:
		<font color=red><pre>&lt;item repeat="0-1"&gt;</pre></font>
		<p>On constate ici que ce paramètre peut être ajouté zéro ou une fois : il s'agit 
		donc d'un paramètre optionnel, et si la phrase prononcée s'arrête à « <i>Sarah, 
		allume la lampe de la cuisine</i> », ça ne constituera donc pas une erreur.</p>
		<p>Mais on peut compléter avec « <i>à cinquante pour cent</i> » :</p>
		<font color=red>
		<pre>&lt;item&gt;à cinquante pour cent		&lt;tag&gt;out.action.ttsDim=" à cinquante pour cent";	out.action.setValue=50;&lt;/tag&gt;&lt;/item&gt;</pre>
		</font>
		<p>La commande initiale (<tt>turnOn</tt>) est donc ignorée, et c'est l'action 
		trouvée ici (<tt>setValue=50</tt>) qui sera envoyée à votre Home Center à la place.</p>
		<p>Enfin, la réponse vocale de SARAH est complétée avec le paramètre
		<tt>ttsDim=" à cinquante pour cent"</tt></p>
</ul>
<p>En résumé, lorsque vous dite : <br />
<i>Sarah, <font color=blue>allume</font> <font color=green> la lampe de la cuisine</font> <font color=red>à 50 %</font></i> <br />
SARAH traduit par : <br />
<tt><font color=blue>turnOn</font> <font color=green>39</font> <font color=red>setValue 50</font></tt> <br />
et confirme son action en répondant : <br />
<font color=blue>j'allume</font> <font color=green>la lampe de la cuisine</font> <font color=red>à 50 %</font></p>

<p>La requête sera formatée pour être envoyée à l'aide de la méthode <tt>POST</tt> à l'url :<br /> 
<tt>http://192.168.1.12:80/api/devices/39/action/setValue</tt> <br />
et les arguments (ici le paramètre 50) seront envoyés au format <tt>json</tt> dans le 
corps de la requête : <br />
<tt>{"args":[50]}</tt>. <br />
Dans le cas ou aucun argument optionnel n'est nécessaire (par exemple pour les commandes
<tt>turnOn</tt> et <tt>turnOff</tt>), le corps de la requête est remplacé par <br />
<tt>{"args":[null]}</tt>.</p>

<p>Note : les explications données ici ne sont qu'une approximation assez éloignée de la 
réalité, notamment sur la temporalité d'analyse des paroles et de formatage des requêtes, 
mais elles ont - je l'espère - le mérite de décrire de manière assez juste la logique de fonctionnement 
de SARAH. </p>
