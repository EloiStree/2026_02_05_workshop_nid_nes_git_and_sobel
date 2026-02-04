## Full Exercise as Video

[https://youtu.be/dLRaB3z-4DQ](https://youtu.be/dLRaB3z-4DQ)

```
Only Controller:
https://github.com/EloiStree/2026_01_18_upm_nes_controller_udp.git

Mini Multiplayer Game:
https://github.com/EloiStree/2020_11_29_upm_udp_thread_in_out_gate.git
https://github.com/EloiStree/2026_01_18_upm_nes_udp_multiplayer.git
```

---

## Présentation de l’exercice

Pour vous présenter l’exercice, il a fallu que je le crée en premier lieu 😉
Vous pouvez trouver les étapes et solutions de création ici.

### **Vidéos : montage de l’exercice**

* Faire un joystick sur Android avec Unity :
  [https://youtu.be/ABI1LT5szes](https://youtu.be/ABI1LT5szes)
* Step by step de la manette NES :
  [https://youtu.be/SHLRBrYZp-s](https://youtu.be/SHLRBrYZp-s)
* Créer un jeu NES multijoueur :
  [https://youtu.be/GlQAoQJvU0c](https://youtu.be/GlQAoQJvU0c)
* Ajouter une façade de code sur la commande NES :
  [https://youtu.be/OrGNXlfH70s](https://youtu.be/OrGNXlfH70s)
* Input mapping sur le contrôleur NES :
  [https://youtu.be/YHsY6f2ZJqA](https://youtu.be/YHsY6f2ZJqA)

---

Sur mon écran sera affiché ce mini-jeu que j’ai fait pour l’occasion :
[<img width="1275" height="662" alt="image" src="https://github.com/user-attachments/assets/f9a4f8ad-a6d9-4cf1-a039-8e07dbc052bc" />](https://github.com/EloiStree/2026_01_18_upm_nes_udp_multiplayer)
* [https://github.com/EloiStree/2026_01_18_unity_nes_multi_controller](https://github.com/EloiStree/2026_01_18_unity_nes_multi_controller)
  * [https://github.com/EloiStree/2026_01_18_upm_nes_udp_multiplayer](https://github.com/EloiStree/2026_01_18_upm_nes_udp_multiplayer)

Celui-ci affiche :

* l’adresse IP : où le trouver sur le réseau
* le port : où trouver l’application sur l’ordinateur

Et attend que vous lui envoyiez des entiers entre **1000 et 2999** via le réseau,
ainsi qu’un deuxième entier pour l’index du joueur que vous voulez contrôler.

Pas de panique 😋, j’ai fait le code pour vous.
Il faut juste les assembler dans Unity autour d’une interface.

*Note : nous allons utiliser **Unity 6000.2**, car la version **6000.3** est cassée depuis décembre 2025.*

---

## Quel chiffre envoyer 🧐 ?

J’utilise un format que j’ai créé appelé **S2W** :
**Scratch to Warcraft** 😋

Voir :
[https://github.com/EloiStree/2024_08_29_ScratchToWarcraft](https://github.com/EloiStree/2024_08_29_ScratchToWarcraft)

Voici les touches pour la NES avec le S2W :

```
Menu Left   1309 2309
Menu Right  1308 2308
Up Arrow    1331 2331
Down Arrow  1335 2335
Left Arrow  1337 2337
Right Arrow 1333 2333
A Button    1300 2300
B Button    1301 2301
```

* **1000** = presser la touche
* **2000** = relâcher la touche

*Notez que dans le S2W, les chiffres de la NES sont basés sur ceux de la Xbox,
ce qui nous permettra de jouer à **TowerFall** dans l’atelier suivant.*

Au final, la NES n’est composée que de :

* un joystick (`DPad` / flèches)
* deux boutons menu
* deux boutons d’action

---

## Simplification de l’exercice

Pour cet exercice, nous pourrions tout recoder afin d’apprendre ce que sont :

* l’UDP
* le réseau
* les singletons
* le binaire
* la compression

Mais nous n’allons pas faire ça.   
  
Si vous voulez faire ca, vous pouvez commencer par ce code:  
```csharp
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
    {
        m_lastException = e.ToString();
    }
}
```

---

# TowerFall et Macro

[<img width="750" height="422" alt="image" src="https://github.com/user-attachments/assets/53339a3c-0d53-4932-ade9-29cb1cbb5fc9" />](https://store.steampowered.com/app/251470/TowerFall_Ascension/?l=french)

[https://store.steampowered.com/app/251470/TowerFall_Ascension/?l=french](https://store.steampowered.com/app/251470/TowerFall_Ascension/?l=french)

Touches du jeu :
[https://github.com/EloiStree/PlayTo_TowerFall](https://github.com/EloiStree/PlayTo_TowerFall)

---



# Full Exercice

https://youtu.be/dLRaB3z-4DQ

If you want a **version française plus formelle**, or a **student-facing handout**, that’s a different cleanup pass.
