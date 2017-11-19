> **Important**
>
> L’activation des bêtas est très très risquée et vous INTERDIT TOUT
> ACCES AU SUPPORT. IL faut aussi obligatoirement passer Jeedom en bêta
> et faire des mises à jour fréquentes même s’il n’y a pas de nouvelles
> mises à jour proposées. IMPORTANT, les versions bêtas sont souvent
> instables et peuvent causer de nombreux soucis, il ne faut surtout pas
> les mettre sur un système de production. EN CAS DE SOUCIS, L’EQUIPE
> JEEDOM NE SERA PAS ET NE POURRA PAS ETRE TENUE POUR RESPONSABLE.

> **Note**
>
> Cette documentation est volontairement peu detaillée pour que
> l’opération ne soit pas facile, en effet être en bêta nécessite des
> compétences aussi bien en informatique qu’en déchiffrage de logs ou
> documentation.

Passer le core en bêta
======================

Il faut d’abord passer Jeedom en version bêta. Pour cela, vérifiez que
vous êtes bien en mode expert, puis dans l’administration de Jeedom,
aller dans "Mises à jour et fichiers". Puis dans l’onglet URL, cliquer
sur activer :

-   URL core Jeedom : <https://github.com/jeedom/core/archive/beta.zip>

-   URL version core Jeedom :
    <https://raw.githubusercontent.com/jeedom/core/beta/core/config/version>

Sauvegarder puis réactualiser la page. Toujours dans la partie "Mises à
jour et fichiers", dans "Source de mise à jour" sélectionner "URL".

Il ne vous reste plus qu'à forcer une mise à jour du core pour passer
sur la version bêta de Jeedom.

> **Important**
>
> Jeedom ne signale en bêta que les mises à jour importantes, il vous
> faudra donc souvent forcer la mise à jour de Jeedom pour avoir la
> dernière version, même si le Centre de mises à jour ne vous propose
> rien.

> **Important**
>
> Une mise à jour en bêta peut complètement casser votre Jeedom et le
> rendre irrécupérable. En cas de soucis vous n’avez AUCUN SUPPORT, il
> faudra vous débrouiller par vos propres compétences.

> **Important**
>
> Le passage d’une version stable à une version bêta est risqué (nous ne
> garantissons jamais que votre Jeedom continue de fonctionne
> correctement). Le passage d’une version bêta à une version stable est
> INTERDIT, en effet, lors d’une telle opération, il est sûr à 100% que
> votre Jeedom sera cassé. Il n’est pas possible de faire un retour
> arrière en restaurant un backup d’une version stable. En cas de
> soucis, vous ne bénéficierez d’AUCUN SUPPORT.

Passer les plugins en bêta
==========================

Pour cela, il faut vous rendre sur le Market Jeedom, puis dans votre
profil, partie "Mon Profil", cocher la case pour avoir accès aux plugins
en version bêta.

Ensuite, dans votre Jeedom, sur les fiches des plugins, vous devriez
voir un bouton "installer bêta".

> **Important**
>
> Tout plugin installé en version bêta interdit l’accès complet au
> support, même si votre problème ne concerne pas ce plugin.

> **Important**
>
> Un plugin en version stable peut ne pas marcher sur un Jeedom bêta.

> **Important**
>
> Un plugin en version bêta NE FONCTIONNE PAS sur un Jeedom stable.

> **Important**
>
> Toute demande de support portant sur comment passer en bêta, ou un
> souci avec un composant en bêta, ou sur un systeme ayant déja eu un
> core ou un plugin en bêta, sera immediatement fermé sans aucune
> réponse de notre part.

