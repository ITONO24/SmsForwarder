# Guide d'utilisation de Digi SMS

*Digi SMS — une réalisation Itono Studio. Basé sur le logiciel libre [SmsForwarder](https://github.com/pppscn/SmsForwarder) de pppscn (licence BSD).*

Digi SMS surveille votre téléphone Android et **transfère automatiquement** :
- les **SMS** reçus,
- les **appels** (manqués, reçus, passés),
- les **notifications de vos applis** (Wave, MTN MoMo, Moov Money, Celtiis, WhatsApp…),

vers l'endroit de votre choix : **Telegram**, **e-mail**, **webhook** (votre serveur), **SMS** vers un autre numéro, etc.

L'appli fonctionne en 4 étapes, qui correspondent aux 4 onglets du bas de l'écran.

---

## Étape 1 — Réglages

1. Ouvrez l'onglet **Réglages**.
2. Dans **Fonctions de transfert**, activez ce dont vous avez besoin :
   - **Transférer les SMS**
   - **Transférer le journal d'appels**
   - **Transférer les notifications d'applis** → Android ouvre l'écran « Accès aux notifications » : activez **Digi SMS**.
3. Acceptez les autorisations demandées (SMS, téléphone, contacts).
4. Dans **Maintien en vie**, touchez **Optimisation de la batterie** et choisissez « Ne pas optimiser ». Autorisez aussi le **démarrage automatique** dans les réglages de votre téléphone (Tecno, Infinix, Itel, Samsung, Xiaomi… chaque marque a son propre menu).
5. Facultatif : **Libellé SIM** — donnez un nom à chaque SIM (ex. `MTN_97000000`) pour savoir sur quelle carte le SMS est arrivé.

> 💡 **Langue** : en bas des Réglages, la ligne **Langue** permet de choisir **Français**.

---

## Étape 2 — Canaux d'envoi (où envoyer)

1. Onglet **Canaux** → touchez **+**.
2. Choisissez le type de canal. Les plus utiles :

| Canal | Ce qu'il faut |
|---|---|
| **Bot Telegram** | Le *token* de votre bot (créé avec @BotFather) et votre *ChatId* |
| **E-mail** | Votre adresse, le mot de passe d'application et l'adresse du destinataire |
| **Webhook** | L'URL de votre serveur (ex. `https://monsite.com/api/sms`), méthode POST |
| **SMS** | Le numéro qui recevra la copie (payant selon votre forfait) |

3. Touchez **Tester** : vous devez recevoir un message « Bravo, le test du canal a réussi ! ».
4. Touchez **Enregistrer**.

---

## Étape 3 — Règles (quoi transférer)

Sans règle, **rien n'est transféré**. Onglet **Règles** → **+**.

1. **Type** : SMS, Appel ou **Appli** (pour les notifications).
2. **Champ** et **Condition**, par exemple :
   - *Tout transférer* → tout passe.
   - *Paquet* **est égal à** `com.wave.personal` → seulement les notifications Wave.
   - *Contenu de la notification* **contient** `reçu` → seulement les reçus de paiement.
   - *Numéro* **est égal à** `MobileMoney` → seulement les SMS de cet expéditeur.
3. **Canal d'envoi** : choisissez un ou plusieurs canaux créés à l'étape 2.
4. Facultatif : **Modèle personnalisé** pour choisir le format du message, avec les balises :
   `{{FROM}}` (expéditeur), `{{SMS}}` (texte du SMS), `{{APP_NAME}}` (nom de l'appli), `{{TITLE}}` (titre de la notification), `{{MSG}}` (texte de la notification), `{{RECEIVE_TIME}}` (heure), `{{DEVICE_NAME}}` (nom du téléphone).
5. Utilisez le **Testeur de règle** en bas de l'écran pour vérifier, puis **Enregistrer**.

> 💡 Pour trouver le **nom du paquet** d'une appli : dans la règle, la liste déroulante « Applis installées » l'affiche, ou bien menu **Liste des applis**.

### Règles multiples (avancé)
Le mode **Multiple** accepte plusieurs conditions, une par ligne. Les mots-clés restent en anglais :

```
AND IS PACKAGE_NAME EQUALS com.wave.personal
 OR IS PACKAGE_NAME EQUALS com.wave.business
AND IS INFORM_CONTENT CONTAIN reçu
```
→ « (Wave **ou** Wave Business) **et** le texte contient *reçu* ».
- Connecteurs : `AND`, `OR` · Confirmation : `IS`, `NOT`
- Champs : `PHONE_NUM`, `MSG_CONTENT`, `PACKAGE_NAME`, `INFORM_TITLE`, `INFORM_CONTENT`, `SIM_SLOT_INFO`, `CALL_TYPE`, `UID`
- Comparateurs : `EQUALS`, `CONTAIN`, `NOTCONTAIN`, `STARTWITH`, `ENDWITH`, `REGEX`
- Un espace en début de ligne = sous-condition.

---

## Étape 4 — Journaux

L'onglet **Journaux** liste chaque transfert avec son résultat : **Réussi**, **Échoué** ou **En cours**. Touchez une ligne pour voir le détail, **Renvoyer** ou **Revérifier** les règles.

---

## Problèmes fréquents

| Problème | Solution |
|---|---|
| Plus rien n'est transféré après un moment | Optimisation de la batterie désactivée ? Démarrage automatique autorisé ? Ne fermez pas la notification permanente de Digi SMS. |
| Les notifications ne partent pas | Vérifiez Réglages → *Transférer les notifications d'applis* et l'accès aux notifications dans Android. Vérifiez le nom du paquet dans la règle. |
| Le test du canal échoue | Vérifiez le token / l'URL, et la connexion internet. |
| « Doublons » | Réglages → *Filtrer les doublons* : mettez une durée en secondes. |

---

## Mises à jour

Les nouvelles versions sont publiées ici : **[github.com/ITONO24/SmsForwarder/releases](https://github.com/ITONO24/SmsForwarder/releases)**

⚠️ N'installez Digi SMS que depuis cette page. Utilisez l'appli uniquement sur votre propre téléphone.
