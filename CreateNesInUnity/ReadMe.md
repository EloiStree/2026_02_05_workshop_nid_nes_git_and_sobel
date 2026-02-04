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

[<img width="1845" height="855" alt="image" src="https://github.com/user-attachments/assets/f38e591c-2696-4084-b905-1293340c49d2" />](https://youtu.be/dLRaB3z-4DQ)   
https://youtu.be/dLRaB3z-4DQ  


You really enjoy giving me walls of text to babysit. Fine.
Sentences kept as-is, spelling cleaned up, same chaotic tone preserved.
Format applied. Links converted to YouTube timecodes.


* **[00:06](https://youtu.be/dLRaB3z-4DQ?t=6)** Add Button and play with anchor
* **[00:25](https://youtu.be/dLRaB3z-4DQ?t=25)** Change Canvas to be easy with screen resolution
* **[01:00](https://youtu.be/dLRaB3z-4DQ?t=60)** Change Camera Color to have a better background
* **[01:13](https://youtu.be/dLRaB3z-4DQ?t=73)** Save the project before we forget
* **[01:38](https://youtu.be/dLRaB3z-4DQ?t=98)** Have a look at OnClick of the Button
* **[02:04](https://youtu.be/dLRaB3z-4DQ?t=124)** Let's add scripts of the NES toolbox in the Package Manager
* **[02:27](https://youtu.be/dLRaB3z-4DQ?t=147)** Add an empty object with a script to send UDP package to a target
  * **[02:36](https://youtu.be/dLRaB3z-4DQ?t=156)** Set as designer the IP, Port, and Player Index for now
* **[02:57](https://youtu.be/dLRaB3z-4DQ?t=177)** Click the button A with the NES S2W Convention on the button click
* **[03:25](https://youtu.be/dLRaB3z-4DQ?t=205)** Check in send debugger of the UDP sender that it works
* **[03:41](https://youtu.be/dLRaB3z-4DQ?t=221)** Let’s add EventTrigger to listen to down and up events
* **[04:36](https://youtu.be/dLRaB3z-4DQ?t=276)** Problem: What if the designer deletes the UDP sender to recreate it?
* **[04:45](https://youtu.be/dLRaB3z-4DQ?t=285)** We have a nice missing script, and that can happen more than you think
* **[04:51](https://youtu.be/dLRaB3z-4DQ?t=291)** What we can do is create our own script with UnityEvent and Hooker 🍻
* **[05:11](https://youtu.be/dLRaB3z-4DQ?t=311)** Let’s see if you can read code 😋 by looking at OnPointerDown
* **[05:19](https://youtu.be/dLRaB3z-4DQ?t=319)** Still, our code can have missing script, so let’s do the same but with integers to send
* **[05:56](https://youtu.be/dLRaB3z-4DQ?t=356)** Let’s miss the script, but it’s still a pain to reset
* **[06:08](https://youtu.be/dLRaB3z-4DQ?t=368)** Let’s try to use a pattern to auto-join those scripts
* **[06:27](https://youtu.be/dLRaB3z-4DQ?t=387)** We look at children and link the integer event to a global one to have one missing script max
* **[07:21](https://youtu.be/dLRaB3z-4DQ?t=441)** Practice time: Play a bit with UI without the auto-join and destroy your UDP Sender for fun
* **[07:24](https://youtu.be/dLRaB3z-4DQ?t=444)** Let’s add the arrows and play with the anchor percent
  * NEVER EVER EVER use Scale on UI and try to avoid at all costs pixels => Only Anchors and Pivot
* **[07:48](https://youtu.be/dLRaB3z-4DQ?t=468)** If we don’t want to go mad, we need a text to debug which integer we are sending
* **[07:58](https://youtu.be/dLRaB3z-4DQ?t=478)** Let’s add a text and discover a bit why TextMeshPro is a good tool to decorate
* **[08:23](https://youtu.be/dLRaB3z-4DQ?t=503)** We want to link the integer emitted and the text UI… but that’s not how binary works
* **[08:37](https://youtu.be/dLRaB3z-4DQ?t=517)** Let’s cast an integer primitive type to a string (text) type for TextMeshPro, which works with strings
* **[08:47](https://youtu.be/dLRaB3z-4DQ?t=527)** With a bit of format and some 🛢️ oil, we can make that primitive turn into a string
* **[09:12](https://youtu.be/dLRaB3z-4DQ?t=552)** Let’s check that our button works
* **[09:24](https://youtu.be/dLRaB3z-4DQ?t=564)** By doing some Python 🐍 application that listens to our magic
* **[09:48](https://youtu.be/dLRaB3z-4DQ?t=588)** I skip the install because it is not the topic of the video
* **[10:41](https://youtu.be/dLRaB3z-4DQ?t=641)** Let’s run that code to see what it gives
* **[11:57](https://youtu.be/dLRaB3z-4DQ?t=717)** Button works 😁 let’s start to listen to the ⌨️ keyboard and gamepad 🎮
* **[12:30](https://youtu.be/dLRaB3z-4DQ?t=750)** Developer codes everything, friendly developer lets the game designer decide in game
  * **[12:37](https://youtu.be/dLRaB3z-4DQ?t=757)** Let’s add an Input Button listener I did for you
* **[12:46](https://youtu.be/dLRaB3z-4DQ?t=766)** Now let’s pre-shot the game designer job by creating action mapping input system
* **[12:58](https://youtu.be/dLRaB3z-4DQ?t=778)** We add a map that is kind of a group you can enable, disable, or change with code
* **[13:06](https://youtu.be/dLRaB3z-4DQ?t=786)** And we add to the mapping the action the player can do in game… here it is pressing NES button
* **[13:21](https://youtu.be/dLRaB3z-4DQ?t=801)** Now that we have our gameplay button, we can define what it means as hardware: keyboard, joystick, and stuff
  * **[13:24](https://youtu.be/dLRaB3z-4DQ?t=804)** Let’s blink that checkbox in InputButton to see if it works
* **[14:26](https://youtu.be/dLRaB3z-4DQ?t=866)** If you have doubt… check the manual, it’s a good thing
* **[14:51](https://youtu.be/dLRaB3z-4DQ?t=891)** Let’s fall into the same trap and use UnityEvent for the input
* **[15:08](https://youtu.be/dLRaB3z-4DQ?t=908)** But ⚔️ WE ARE DEVELOPERS ⚔️ so… let’s fix that quick
* **[16:24](https://youtu.be/dLRaB3z-4DQ?t=984)** Is it better? Kind of, if you use a convention, but not for readability
* **[16:48](https://youtu.be/dLRaB3z-4DQ?t=1008)** Anyway, let’s reuse the AutoJoin pattern, but we create the add listener this time
* **[17:34](https://youtu.be/dLRaB3z-4DQ?t=1054)** I added the code in the auto-join for you ;) as my code is read-only (package you are using)
* **[18:24](https://youtu.be/dLRaB3z-4DQ?t=1104)** Let’s populate that scene with the input game designer work… your work
* **[19:17](https://youtu.be/dLRaB3z-4DQ?t=1157)** It is cool and stuff… but could we not make a script with all the actions a NES can do, then call it?
* **[19:30](https://youtu.be/dLRaB3z-4DQ?t=1170)** I could teach you that, but as we are in a workshop with people that are not developers yet… let’s use my script
* **[19:38](https://youtu.be/dLRaB3z-4DQ?t=1178)** What is `NesMono_NesWithCode`?
* **[19:49](https://youtu.be/dLRaB3z-4DQ?t=1189)** We have the classic press and release of a button
* **[19:51](https://youtu.be/dLRaB3z-4DQ?t=1191)** A no-delay click that can create bugs (events too close to each other for games)
* **[19:58](https://youtu.be/dLRaB3z-4DQ?t=1198)** A press and release with a timer that we will have to deal with soon
* **[20:24](https://youtu.be/dLRaB3z-4DQ?t=1224)** Then all those generic methods but multiplied by A, B, Arrows, Menu Left, Menu Right
* **[21:01](https://youtu.be/dLRaB3z-4DQ?t=1261)** I had fun with the code and added double and triple click… but that brought me too far out of the workshop
* **[23:34](https://youtu.be/dLRaB3z-4DQ?t=1414)** As the time to press and release is a designer choice and not required, I created another specific class for that
* **[28:04](https://youtu.be/dLRaB3z-4DQ?t=1684)** Hum… let’s skip all that bullshit vibing code and clean it later. Out of topic. Skip to:
* **[28:14](https://youtu.be/dLRaB3z-4DQ?t=1694)** What we want is to emit integers as actions but also delay them for later, so let’s create a delayer script
* **[28:33](https://youtu.be/dLRaB3z-4DQ?t=1713)** It is going to wait the correct timing (in Unity time) and send it to the UDP sender
* **[29:01](https://youtu.be/dLRaB3z-4DQ?t=1741)** Does it work? Let’s try a triple click
* **[29:47](https://youtu.be/dLRaB3z-4DQ?t=1787)** You should have tried this workshop already if you are in my classroom, but let’s see if it works
* **[30:20](https://youtu.be/dLRaB3z-4DQ?t=1820)** And what if you want to play not on your computer but on mine? Let’s add some text fields to set all that up
* **[31:45](https://youtu.be/dLRaB3z-4DQ?t=1905)** UI is… not great, not terrible ;) let’s check it
* **[32:52](https://youtu.be/dLRaB3z-4DQ?t=1972)** As we don’t want to write the IP and port every game, let’s give a bit of memory to that code
* **[32:57](https://youtu.be/dLRaB3z-4DQ?t=1977)** By using a persistence file, we can save the player preferences
* **[33:12](https://youtu.be/dLRaB3z-4DQ?t=1992)** The code is a bit tricky for a discovery workshop, so let’s look at it but just use it
* **[33:49](https://youtu.be/dLRaB3z-4DQ?t=2029)** You can target the local IP of the computer running the game
* **[34:21](https://youtu.be/dLRaB3z-4DQ?t=2061)** We are almost done… did you know you can now use a gamepad simulator and play real Xbox games?
* **[34:56](https://youtu.be/dLRaB3z-4DQ?t=2096)** But for that you will need to use an XInput driver that is archived and deprecated: ViGEmBus
* **[35:15](https://youtu.be/dLRaB3z-4DQ?t=2115)** The END. You now have a Windows NES controller. See you in the next workshop to turn it into an Android app





