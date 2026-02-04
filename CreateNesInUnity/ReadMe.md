Pour vous presentez l exercice, il a fallu que je le creer en premier lieu ;)  
Vous pouvez trouvez les etapes et solutions de creation ici;  

**Vidéo : montage de l’exercice :**

* Faire un joystick sur Android avec Unity: [https://youtu.be/ABI1LT5szes](https://youtu.be/ABI1LT5szes)
* Step by step de la manette NES : [https://youtu.be/SHLRBrYZp-s](https://youtu.be/SHLRBrYZp-s)
* Créer un jeu NES multijoueur : [https://youtu.be/GlQAoQJvU0c](https://youtu.be/GlQAoQJvU0c)
* Ajouter une façade de code sur la commande NES : [https://youtu.be/OrGNXlfH70s](https://youtu.be/OrGNXlfH70s)
* Input mapping sur le contrôleur NES : [https://youtu.be/YHsY6f2ZJqA](https://youtu.be/YHsY6f2ZJqA)

----------------------

Sur mon ecran sera afficher ce mini jeu que j ai fait pour l occasion:
[<img width="1275" height="662" alt="image" src="https://github.com/user-attachments/assets/f9a4f8ad-a6d9-4cf1-a039-8e07dbc052bc" />
](https://github.com/EloiStree/2026_01_18_upm_nes_udp_multiplayer)   
- https://github.com/EloiStree/2026_01_18_unity_nes_multi_controller  
  - https://github.com/EloiStree/2026_01_18_upm_nes_udp_multiplayer
 
Celui-ci affiche son:
- address IP : ou le trouver sur le reseau
- port : ou trouver l application sur l ordinateur

Et attends que vous lui envoyer des entiers entre 1000 - 2999 via le reseau.   
Et un deuxieme entier pour l index du joueur que vous voulez jouer.   

Pas de panique 😋, j ai fait le code pour vous.
Il faut juste les assembers dans Unity autour d un interface.

_Note: nous allons utiliser Unity 6000.2 car le 6000.3 est exploser en decembre 2025._

**Quel chiffre envoyer 🧐?**

J utilise un format que j ai creer appeler les S2W:  
Scratch to Warcraft 😋.  

Voir: https://github.com/EloiStree/2024_08_29_ScratchToWarcraft   

Voici les touches pour la NES avec le S2W:  
```
 Menu Left 1309 2309
 Menu Right 1308 2308
 Up Arrow 1331 2331
 Down Arrow 1335 2335
 Left Arrow 1337 2337
 Right Arrow 1333 2333
 A button 1300 2300
 B button 1301 2301
```
1000 = Presser la touche
2000 = Relacher la touche.

_Noter que dans le S2W, les chiffre de la NES sont sur celui de la Xbox.
Ce qui nous permettra des jouers a tower fall dans l ateliers suivant._

Au final la NES n est que composer de 
* un joystick (`DPad`/ Arrow)
* deux menus
* deux boutons


-------------------------------------------

Pour cette exercice nous pourrions tout recoder pour apprendre ce qu est:
UDP, le reseaux, les singletons, le binaire, la compression ...
On va pas faire ca.

Donc nous allons ajouter ma boite a outil a votre projet Unity.
Git: https://github.com/EloiStree/2026_01_18_upm_nes_controller_udp.git
<img width="1305" height="353" alt="image" src="https://github.com/user-attachments/assets/15bb1e34-d501-4616-88cc-8e699441f450" />

Vous devriez avoir dans le package Unity un outil en plus.
<img width="1140" height="1032" alt="image" src="https://github.com/user-attachments/assets/16f634b1-c869-4724-ab2a-3daf25eb1b1a" />
Donc la solution si vous vous perdez en chemin.


Ok, nous on veut pouvoir tourner a gauche.
Et donc envoyer `Left Arrow` via le reseau pour le joueur 6.


Il nous faut donc ajouter un Button UI de Unity.  
_(Et importer TextMesh Pro par default)_
[Demo Video]

On peu constater que le bouton Unity n a que un unity even OnClick.
Ce qui n est pas pratiquer...

Mais  nous avons un script `EventTrigger` qui nous permettres d avoir plus evenement a ecouter.
Ecoutons a PointerDown et PointerUp
[Demo Video]

On peut ecouter au bouton, il nous faut donc ajouter un code a appeler.
Ajouter un point vide dans la scene avec le script `NesMono_SendBytesToUdpTargetS2W`

Il posed deux methodes:
```
//!
public void SendIndexIntegerToUdpTarget(int intValue)
{
    SendIndexIntegerToUdpTarget(m_playerDefaultIndex, intValue);
}

public void SendIndexIntegerToUdpTarget(int index, int intValue)
{
    byte[] doubleInt = new byte[8];
    byte[] first = BitConverter.GetBytes(index);
    byte[] second = BitConverter.GetBytes(intValue);
    Buffer.BlockCopy(first, 0, doubleInt, 0, 4);
    Buffer.BlockCopy(second, 0, doubleInt, 4, 4);
    SendBytesTo(m_targetIpv4, m_targetPort, doubleInt);
    m_onIntegerValueSent?.Invoke(intValue);
    m_onIndexValueIntegerSent?.Invoke(index, intValue);
}
public void SendBytesTo(string ip, int port, byte[] sendBytes)
{
    if (sendBytes == null || sendBytes.Length == 0)
        return;
    try
    {
        using (var client = new UdpClient())
        {
            m_sendToDebug = sendBytes;
            client.Send(sendBytes, sendBytes.Length, ip, port);
        }
    }
    catch (SocketException e)
    {m_lastException = e.ToString();}
}
```

Elle permet de dire, les autres developpeurs configurer l'IP, le port et le jouer.
Et envoie moi sur le reseau cette entier la bas avec l index du joueur.

Le script, oui convertit l index et la valeur en binaire et l envoie au reseau.







---------------------

# Tower Fall et Macro

[<img width="750" height="422" alt="image" src="https://github.com/user-attachments/assets/53339a3c-0d53-4932-ade9-29cb1cbb5fc9" />](https://store.steampowered.com/app/251470/TowerFall_Ascension/?l=french)  
https://store.steampowered.com/app/251470/TowerFall_Ascension/?l=french

Les touches du jeu:  
https://github.com/EloiStree/PlayTo_TowerFall  




